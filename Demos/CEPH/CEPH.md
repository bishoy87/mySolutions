# This solution initially started in the cloud. However, since it is a demo, management has requested that it be self-hosted to reduce costs.
# this brief for CEPH solution:
**Ceph is an open-source storage platform designed to provide scalable and reliable storage solutions. It integrates object, block, and file storage into a unified system, offering high performance and fault tolerance,The implment will be a master and 2 nodes ,I used vagrant to up all VMs with using inline shell to make the installation**
```console
Vagrant.configure("2") do |config|
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.hostmanager.manage_guest = true
	config.hostmanager.ignore_private_ip = false
	config.hostmanager.include_offline = true
    
    config.vm.define "ceph1" do |ceph1|
	ceph1.vm.hostname = "ceph1"
    ceph1.vm.box = "generic/centos8"
    ceph1.vm.network "public_network"  , bridge: ["Intel(R) Wireless-AC 9560 160MHz"]
    ceph1.vm.network "private_network", ip: "192.168.56.10"
    ceph1.vm.provision "shell", inline: <<-SHELL
	 sudo yum update -y
	 sudo yum install curl -y
	 sudo systemctl stop firewalld
	 sudo systemctl disable firewalld
	 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     sudo yum install docker-ce docker-ce-cli containerd.io -y
	 sudo systemctl enable docker
	 sudo systemctl start docker
	 cd /tmp
	 curl --silent --remote-name --location https://github.com/ceph/ceph/raw/pacific/src/cephadm/cephadm
     chmod +x cephadm
	 sudo ./cephadm add-repo --release pacific
	 sudo ./cephadm install 
     sudo cephadm bootstrap --mon-ip 192.168.56.10  &> result.log
	 SHELL
    end
	
	
    config.vm.define "ceph2" do |ceph2|
	ceph2.vm.hostname = "ceph2"
    ceph2.vm.box = "generic/centos8"
    ceph2.vm.network "public_network"  , bridge: ["Intel(R) Wireless-AC 9560 160MHz"]
    ceph2.vm.network "private_network", ip: "192.168.56.11"
	ceph2.vm.provision "shell", inline: <<-SHELL
	 sudo yum install -y ca-certificates curl gnupg lsb-release
	 sudo systemctl stop firewalld
	 sudo systemctl disable firewalld
	 sudo yum install -y yum-utils
	 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     sudo yum install docker-ce docker-ce-cli containerd.io -y
	 sudo systemctl enable docker
	 sudo systemctl start docker
	 sudo usermod -aG docker $USER
     SHELL
    end
	
    config.vm.define "ceph3" do |ceph3|
	ceph3.vm.hostname = "ceph3"
    ceph3.vm.box = "generic/centos8"
    ceph3.vm.network "public_network"  , bridge: ["Intel(R) Wireless-AC 9560 160MHz"]
    ceph3.vm.network "private_network", ip: "192.168.56.12"  
	ceph3.vm.provision "shell", inline: <<-SHELL
	 sudo yum install -y ca-certificates curl gnupg lsb-release
	 sudo systemctl stop firewalld
	 sudo systemctl disable firewalld
	 sudo yum install -y yum-utils
	 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     sudo yum install docker-ce docker-ce-cli containerd.io -y
	 sudo systemctl enable docker
	 sudo systemctl start docker
	 sudo usermod -aG docker $USER
	 SHELL
    end
  end
```






Refernce:
https://gist.github.com/kalaspuffar/92e8108c472f367be28c31f3bd29f5ef