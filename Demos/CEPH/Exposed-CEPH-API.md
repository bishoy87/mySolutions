**After installing the self-hosted CEPH cluster on-premises, we encountered an issue granting access to the development team to start their work. The installation wasn't done on the testing environment due to IT restrictions, and instead, it was done on a laptop. To resolve this situation, I implemented an SSH reverse tunnel using a EC2 in AWS Cloud.**

#The Solution Steps
we have the following servers:
1- Laptop which is hosted the CEPH Cluster
2- CEPH VMs
3- VM on Azure Cloud

# Run this on the CEPH Master VM
```console
ssh -i "azurevmkey.pem" -N -R 8081:\*:8091 ec2user@<ec2publicip>
```

azurevmkey.pem : EC2 key
-R : reverse tunnel
-N : none interactive session
8081 : tunnel port
8091 : cephgateway port which is that we need to expose
<azurepublicip> : EC2 Elastic IP

# Create this command in bash then create systemd service to be sure it will be still up and running and will restart if crashed
**Bash script:**
```console
#! /bin/bash
while true
do
   ssh -i "rtceph.pem" -N -R 8081:\*:8091 ec2-user@34.194.38.142
   echo "Something wen wrong, Restarting"
   sleep 2
done	
```


```markdown
sudo vi /etc/systemd/system/ssh-tunnel.service
```
```console
[Unit]
Description=ssh tunnel

[Service]
Type=simple
ExecStart=/bin/bash /tmp/ssh-tunnel.sh
WorkingDirectory=/tmp
Restart=on-failure
RestartSec=2

[Install]
WantedBy=multi-user.target
```

# As work on this solution was done intermittently, we terminated the EC2 instance to reduce costs. However, to be prepared in case we need to reinitiate the solution, we created a Terraform script as shown below:


```console
provider "aws" {
  region = "us-east-1"
}

#######################################
#Security Group
###################################
resource "aws_security_group" "EC2_tunnel_SG" {
  name        = "EC2_tunnel_SG"
  description = "EC2_tunnel_SG"
  vpc_id      = "vpc-05106c9895a78764b"

  ingress {
    description      = "ssh from VPC"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"] 

  }
  
    ingress {
    description      = "ssh from VPC"
    from_port        = 8081
    to_port          = 8081
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"] 

  }


  tags = {
    Name = "EC2_tunnel_SG"
  }
}

##################
#EC
######################

data "aws_key_pair" "existing_key" {
  key_name = "rtceph"
}

resource "aws_instance" "SSH_R_Tunnel" {
  ami = "ami-05c13eab67c5d8861"
  instance_type = "t3.micro"
  vpc_security_group_ids = [aws_security_group.EC2_tunnel_SG.id]
  key_name = data.aws_key_pair.existing_key.key_name
  subnet_id = "subnet-062ee1b8a7b3a761a"
  associate_public_ip_address = true
  tags = {
    Name="SSH_R_Tunnel"

  }
  root_block_device {
    delete_on_termination = true
  }
  
  provisioner "remote-exec" {
    inline = [
	  "sudo sh -c 'echo GatewayPorts yes >> /etc/ssh/sshd_config'",
      "sudo systemctl restart sshd"
    ]

    connection {
      type        = "ssh"
      user        = "ec2-user"  # Replace with the appropriate SSH user for your AMI
      private_key = file("C:/Users/brizk/Downloads/rtceph.pem")
      host        = self.public_ip
    }
  }
}



output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.SSH_R_Tunnel.public_ip
}

```

# the below script to update the ssh-tunnel.sh with the new ec2 ip
```console
#!/bin/bash
# Ask the user for the replacement text
read -p "Enter the replacement text: " NewIP
sed -i "s/\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}/$NewIP/g" ssh-tunnel.sh
echo "IP replaced successfully new IP is $NewIP."
systemctl restart ssh-tunnel
```