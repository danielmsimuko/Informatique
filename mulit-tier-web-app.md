---
sidebar_position: 1
title: 1.1 Designing a Multi-Tier Web Application
---
import ReactPlayer from 'react-player'
import MyVideoUrl from './project-files/00_multi-tier-web-app_terr_provisioner.mp4';

This is an all encompassing project on application planning, design and deployment.

We are using our build knowledge thus far to create a full stack web application. 

:::info We are going to:
1. Create **five unique virtual machines** using **terraform**. Each will serve a different purpose. 
2. **Install** programs onto virtual machines using **terraform provisioners** and **ansible**
:::

## Rundown of Project 

The project consists of five virtual machines which will run different services. 

1. Nginx **Web Server**
2. TomCat **Application Server**
3. RabbitMQ **Broker/Queuing Agent**
4. Memcache **DB Caching**
5. MySQL **Database**

Further explanation: 

First part of the project will be to create the first set of servers for our project. 

### MySQL Server

1. Write `` main.tf `` code for virtual machine `` sql-db-vm `` 

:::note
All code snippets are incomplete due to length of files. For full project files, email: *dmsimuko@gmail.com* 
:::

Below is the attached code snippet for the creation of the mysql virtual machine. 

````terraform
resource "azurerm_linux_virtual_machine" "sql-db-vm" {
  name                            = "sql-db-vm"
  resource_group_name             = var.rg-name
  location                        = azurerm_resource_group.fullStackApp-rg.location
  size                            = "Standard_B1s"
  admin_username                  = "username"    # Replace with your desired username
  admin_password                  = "password1" # Replace with your desired password
  disable_password_authentication = false
  network_interface_ids           = [
    azurerm_network_interface.sql-db-nic.id,
  ]
}
````

2. Code files should include remote-exec provisioner instructions. Code snippet below installs ansible and mariadb on the virtual machine.  

````shell
  connection {
    host     = azurerm_linux_virtual_machine.sql-db-vm.public_ip_address
    type     = "ssh"
    user     = "username"    # Replace with the username for your VM
    password = "password1" # Replace with the password for your VM
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install -y ansible",
      "sudo apt install git mariadb-server -y",
      "sudo systemctl start mariadb",
      "sudo systemctl enable mariadb"
    ]
  }
````

3. For the virtual machine to be created, we will validate this file using `` terraform validate `` and then apply changes if configuration is valid. 

````shell
terraform apply --auto-approve 
````

4. Once creation of resources is successful, login to virtual machine to verify mariaDB and ansible installation.

````shell
ssh username@vm-ip-addr

sudo systemctl status mariadb
````

Let us now do the same with the memcache server.

### Memcache Server

1. Include into `` main.tf `` code for creation of the memcache server named `` mem-db-server ``

````terraform
resource "azurerm_linux_virtual_machine" "mem-db-vm" {
  name                            = "mem-db-vm"
  resource_group_name             = var.rg-name
  location                        = azurerm_resource_group.fullStackApp-rg.location
  size                            = "Standard_B1s"
  admin_username                  = "username"    # Replace with your desired username
  admin_password                  = "password1" # Replace with your desired password
  disable_password_authentication = false
  network_interface_ids = [
    azurerm_network_interface.mem-db-nic.id,
  ]

  connection {
    host     = azurerm_linux_virtual_machine.mem-db-vm.public_ip_address
    type     = "ssh"
    user     = "username"    # Replace with the username for your VM
    password = "password1" # Replace with the password for your VM
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install memcached -y",
      "sudo systemctl start memcached",
      "sudo systemctl enable memcached"
    ]
  }
}
````
2. Again, we will include code to access the virtual machine and install memcache.

3. Format code using `` terraform fmt `` and then apply changes using `` terraform apply``. 

When all is ready, you should have two virtual database machines ready for configuration 

4. You can test the configuration by checking the status of the services for each machine. Login to ``` mem-db-vm ``` and enter command: 

````shell
sudo systemctl status memcached

memcached.service - memcached daemon
     Loaded: loaded (/lib/systemd/system/memcached.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-08-04 14:04:41 UTC; 3min 51s ago
````

5. We can do the same with ``` sql-db-vm ``` 

````shell
 sudo systemctl status mariadb

mariadb.service - MariaDB 10.3.38 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-08-04 14:06:12 UTC; 5min ago
````

We are now ready to continue with our second set of virtual machines. 

Our second set of servers are to be created for running NGINX, RabbitMQ and TomCat. 

### NGINX Server 

1. Below is the code to create the virtual machine server with remote-exec provisioned to install nginx on the server. 

````terraform
resource "azurerm_linux_virtual_machine" "nginx-vm" {
  name                            = "nginx-vm"
  resource_group_name             = var.rg-name
  location                        = azurerm_resource_group.fullStackApp-rg.location
  size                            = "Standard_B1s"
  admin_username                  = "username"    # Replace with your desired username
  admin_password                  = "Password1" # Replace with your desired password
  disable_password_authentication = false
  network_interface_ids           = [
    azurerm_network_interface.nginx-vm-nic.id,
  ]

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install nginx -y"
    ]
  }
}
````

2. After validating the code and applying the changes to the file creates the nginx server. 

````shell
sudo systemctl status nginx 

nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-08-07 10:40:01 UTC; 4min 39s ago
````


### TomCat Server

1. Do the same with the TomCat Server. Create the code base for a virtual machine and the remote exec provisioner to 
automatically install tomcat9 

````terraform
resource "azurerm_linux_virtual_machine" "tomcat-vm" {
  name                            = "tomcat-vm"
  resource_group_name             = var.rg-name
  location                        = azurerm_resource_group.fullStackApp-rg.location
  size                            = "Standard_B1s"
  admin_username                  = "username"    # Replace with your desired username
  admin_password                  = "password1" # Replace with your desired password
  disable_password_authentication = false
  network_interface_ids           = [
    azurerm_network_interface.tomcat-vm-nic.id,
  ]

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt upgrade -y",
      "sudo apt install openjdk-11-jdk -y",
      "sudo apt install tomcat9 tomcat9-admin tomcat9-docs tomcat9-common git -y"
    ]
  }
}
````

2. Confirm the installation on the server using the systemctl command. 

````shell
suco systemctl status tomcat9

tomcat9.service - Apache Tomcat 9 Web Application Server
     Loaded: loaded (/lib/systemd/system/tomcat9.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-08-07 11:21:26 UTC; 2min 25s ago
````

We can now install the third server to complete our virtual environment. 

### RabbitMQ Server

1. Let us create the last server in our environment. This is the rabbitmq server. 

````terraform
resource "azurerm_linux_virtual_machine" "rabbit-vm" {
  name                            = "rabbit-vm"
  resource_group_name             = var.rg-name
  location                        = azurerm_resource_group.fullStackApp-rg.location
  size                            = "Standard_B1s"
  admin_username                  = "username"    # Replace with your desired username
  admin_password                  = "password" # Replace with your desired password
  disable_password_authentication = false
  network_interface_ids           = [
    azurerm_network_interface.rabbit-vm-nic.id,
  ]

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt purge erlang*",
      "sudo wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -",
      "sudo apt-get install rabbitmq-server -y"
    ]
  }
}
````

Once the config file has been updated, you can run terraform apply and auto approve the build

2. A virtual machine should be created with rabbitmq automatically installed using the remote provisioner.  

````shell
sudo systemctl status rabbitmq-server

rabbitmq-server.service - RabbitMQ Messaging Server
     Loaded: loaded (/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-08-07 13:03:10 UTC; 1min 29s ago
````

Let us use our current infrastructure to now deploy the application:

Here is a little snippet of what you should expect if you have used the code correctly: 

<ReactPlayer playing="true" muted="true" width="1080" height="720" controls url={MyVideoUrl}/>
