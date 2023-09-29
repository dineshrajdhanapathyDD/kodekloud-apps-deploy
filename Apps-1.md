# Introduction

This is a sample e-commerce application built process. 

Let us now setup the complete app - both web and database - on a single
server. Install mariadb and start the process with default conf. Ensure
that process is started on server reboot.

## Deploy and Configure Database

1. Install MariaDB

```
sudo yum install -y mariadb-server
sudo systemctl start mariadb
sudo service mariadb start
sudo systemctl enable mariadb
```

## hint:
Run the command: sudo yum install -y mariadb-server to install
Run the command: sudo systemctl start mariadb to start the process
Run the command: sudo systemctl enable mariadb to ensure that process is
started on server reboot.

## Configure Database

DATABASE : ecomdb
USER : \'ecomuser\'@\'localhost\'
PASSWORD: ecompassword
PRIVILEGES : ALL PRIVILEGES ON \*.\* TO \'ecomuser\'@\'localhost\'

Note: Run sudo mysql command to get into MYSQL shell.

Run sudo mysql command to get into MYSQL shell then run below
commands (in capital alphabets)

```
$ mysql
MariaDB > CREATE DATABASE ecomdb;
MariaDB > CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
MariaDB > GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
MariaDB > FLUSH PRIVILEGES;
```
> ON a multi-node setup remember to provide the IP address of the web server here: `'ecomuser'@'web-server-ip'`


**Load Product Inventory Information to database**

We have downloaded Product Inventory Information db-load-script.sql
in /opt directory. Load /opt/db-load-script.sql in mysql

Run 
```
sudo mysql \< /opt/db-load-script.sql
```

## Deploy and Configure Web

**Install below packages**

httpd

php

php-mysqlnd

```
sudo yum install -y httpd php php-mysqlnd
```
 

**Configure httpd**

Make the php page the default page for httpd.

Update the httpd.conf file to use index.php instead of index.html

```
sudo sed -i \'s/index.html/index.php/g\'
/etc/httpd/conf/httpd.conf
```

**Start httpd**

Start and enable httpd

```
sudo systemctl start httpd ; sudo systemctl enable httpd
```

**Download code**

Download code with git. Install git if necessary

GIT repo URL :
https://github.com/kodekloudhub/learning-app-ecommerce.git

target directory : /var/www/html/

```
sudo yum install -y git; sudo git clone
https://github.com/kodekloudhub/learning-app-ecommerce.git
/var/www/html/
```

**Update index.php**

Since we have both the web and database on the same server, update
the IP address configured in index.php file to localhost.

Change 172.20.1.101 to localhost in index.php file.

aUse VI editor to edit the index.php file OR run the command 

```
sudo sed -i \'s/172.20.1.101/localhost/g\' /var/www/html/index.php
``` 
to update IP.

**Test**

Please check default apache content by clicking tab called ecommerce
Application on the top right of the terminal.

Apache is listening on port 80 by default.

check the port for the deploy apps.
