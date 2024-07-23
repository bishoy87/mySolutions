# the installation of vaultwarden will be with docker with maraia db without docker

# install and confgure marai DB
sudo apt update
sudo apt install mariadb-server
sudo mysql_secure_installation
# Configure MariaDB
By default, MariaDB allows connection only from localhost, all connections from a remote server are denied by default.
The first thing you need to do is to configure the MariaDB server to listen to all IP addresses on the system.
You can do it by editing the MariaDB default configuration file. Look for "bind-address" directive in these two locations (make the change in whichever file you find that directive). You can open the file using your favorite text editor:

```console
nano /etc/mysql/my.cnf
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

bind-address = 0.0.0.0

Change the value of the bind-address from 127.0.0.1 to 0.0.0.0 so that MariaDB server accepts connections on all host IPv4 interfaces.

sudo systemctl restart mariadb

# Grant Access to a User from a Remote System
In this section, we will create a new database named wpdb and a user named wpuser, and grant access to the remote system to connect to the database wpdb as user wpuser.
First, log in to the MariaDB shell with the following command:
mysql -u admin -p

Provide your admin (root) password as shown in the Webdock backend and when you get the prompt create a database and user with the following command:

MariaDB [(none)]> CREATE DATABASE vaultwarden;
MariaDB [(none)]> CREATE USER  'mysqlget'@'localhost'IDENTIFIED BY 'password';

Next, you will need to grant permissions to the remote system with IP address 208.117.84.50 to connect to the database named wpdb as user wpuser. You can do it with the following command:

MariaDB [(none)]> GRANT ALL ON wpdb.* to 'wpuser'@'208.117.84.50'IDENTIFIED BY 'password'WITH GRANT OPTION;

Next, flush the privileges and exit from the MariaDB shell with the following command:

MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> EXIT;


A brief explanation of each parameter is shown below:
    • wpdb: It is the name of the MariaDB database that the user wants to connect to.
    • wpuser: It is the name of the MariaDB database user.
    • 208.117.84.50: It is the IP address of the remote system from which the user wants to connect.
    • password: It is the password of the database user.
If you want to grant remote access on all databases for wpuser, run the following command:

MariaDB [(none)]> GRANT ALL ON *.* to 'wpuser'@'208.117.84.50'IDENTIFIED BY 'password'WITH GRANT OPTION;

If you want to grant access to all remote IP addresses on wpdb as wpuser, use % instead of IP address (208.117.84.50) as shown below:

MariaDB [(none)]> GRANT ALL ON wpdb.* to 'wpuser'@'%'IDENTIFIED BY 'password'WITH GRANT OPTION;

If you want to grant access to all IP addresses in the subnet 208.117.84.0/24 on wpdb as user wpuser, run the following command:
MariaDB [(none)]> GRANT ALL ON wpdb.* to 'wpuser'@'208.117.84.%'IDENTIFIED BY 'password'WITH GRANT OPTION;


# vault warden dockerfile
version: "3.7"
services:
 vaultwarden:
  image: "vaultwarden/server:latest"
  container_name: "vaultwarden"
  hostname: "vaultwarden"
  restart: always
  volumes:
   - "vaultwarden_vol:/data/"
  environment:
   - "DATABASE_URL=mysql://mysqlget:password@192.168.56.21/vaultwarden"
   - "ADMIN_TOKEN=H7jbcZJqQG5SXbMUv/Yo+4fXnynZ4pfHj/62c2q93pJlgtc7ls8wGKotCGPAjXaZ"
   - "RUST_BACKTRACE=1"
  ports:
   - "8080:80"


# Enabling admin page
openssl rand -base64 48
docker run -d --name vaultwarden -e ADMIN_TOKEN=LiDONkdWRbN9dVA6O7V76q5Q1GAie9MQ7ipTqJl9VE8xamZ6CBsx7/pwNR5SLRjr -v /vw-data/:/data/  -p 800:80  vaultwarden/server:latest

# refrence: 
https://github.com/dani-garcia/vaultwarden 
https://github.com/dani-garcia/vaultwarden/wiki/Enabling-admin-page 