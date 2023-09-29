# Introduction

This is a sample e-commerce application built process. 

Let us get some more practice. Let\'s try to do the same, but in a
distributed setup. We will install database on db server, and website on
web server. Install mariadb and start the process with default conf.
Ensure that process is started on server reboot.

## Deploy and Configure Database and target server

**Target server: db**
ssh db to login to db server; 

```
ssh db
sudo yum install -y mariadb-server 
sudo systemctl start mariadb
sudo systemctl enable mariadb 
```

![a1](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/318db13b-7cf8-48ae-8f0a-6fd43e1a4b48)

![a2](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/78d7c5eb-163e-4b36-aa58-11b77dd9ce7c)


## Configure Database

Configure the Database as below. To allow remote connection from any host, we will use the % operator in the host field of the user.

target server: db

DATABASE : ecomdb
USER : \'ecomuser\'@\'%\'
PASSWORD: ecompassword
PRIVILEGES : ALL PRIVILEGES ON \*.\* TO \'ecomuser\'@\'%\'

Run sudo mysql command on db server to get into the MYSQL shell
then run the below commands (in the capital alphabet)

```
sudo mysql
MariaDB \> CREATE DATABASE ecomdb;
MariaDB \> CREATE USER \'ecomuser\'@\'%\' IDENTIFIED BY\'ecompassword\';
MariaDB \> GRANT ALL PRIVILEGES ON \*.\* TO \'ecomuser\'@\'%\';
MariaDB \> FLUSH PRIVILEGES;
```

![a3](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/67b174ed-5ffe-484d-9694-9d7ca1a80939)

**Load Product Inventory Information to database**

We have downloaded Product Inventory Information db-load-script.sql in /opt directory. 
Load /opt/db-load-script.sql in mysql.

target server: db

Run 

```
sudo mysql \< /opt/db-load-script.sql
```

![a4](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/db49df44-22ff-4ed1-ba27-3b9887583a14)

## Deploy and Configure Web

**Install below packages**

We are done with the database setup. Let us now setup the web server.
Go to web and install below packages

httpd

php

php-mysqlnd

target server: web

Go back to the web server. 

```
sudo yum install -y httpd php php-mysqlnd
```

![a5](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/75992310-8eec-425a-8281-1f817381d139)

![a6](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/90b16e9a-25a2-45e7-8968-bb57059438a4)


**Configure httpd**

Make the php page the default page for httpd.

Configure httpd for index.php

```
sudo sed -i \'s/index.html/index.php/g\' /etc/httpd/conf/httpd.conf
```

![a7](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/96067632-b773-404c-856e-3c7796017816)

**Start httpd**

Start and enable the httpd service.

```
sudo systemctl start httpd; sudo systemctl enable httpd
```

![a8](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/d4ea2522-a00e-4be2-a879-874106991c52)

**Download code**

Download code from the git command. Install git if it\'s not available on the web server.

GIT repo URL: https://github.com/kodekloudhub/learning-app-ecommerce.git

target directory: /var/www/html/

target server: web

```
sudo yum install -y git; sudo git clone https://github.com/kodekloudhub/learning-app-ecommerce.git
/var/www/html/
```

![a9](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/4b0e82bf-8ec2-4030-b58a-e6b93852f472)

**Update index.php**

Let us now point the web server to the database server. Update
index.php to replace the default IP address of the database server to
point to db.

Change 172.20.1.101 to db in index.php file.

Use vi editor to update the file or run the command 

```
sudo sed -i \'s/172.20.1.101/db/g\' /var/www/html/index.php
``` 
to update the IP address.

![a10](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/61f870b0-48e1-42e3-9ebb-b0cdc20f52bf)

**Deploy webapp**

Check the default apache content by clicking on the tab called E-commerce Application on the top right of the terminal.

Apache web server is listening on port 80 by default.

![a11](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/69f564bd-b04f-4bb7-ac64-4ab02e0e16a0)



