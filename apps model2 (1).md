# Introduction

This is a sample e-commerce application built process. 

Let us get some more practice. Let\'s try to do the same, but in a
distributed setup. We will install database on db server, and website on
web server. Install mariadb and start the process with default conf.
Ensure that process is started on server reboot.

## Deploy and Configure Database and target server

**Target server: db**

```
ssh db to login to db server; sudo yum install -y mariadb-server 
sudo systemctl start mariadb
sudo systemctl enable mariadb 
```

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
MariaDB \> CREATE DATABASE ecomdb;
MariaDB \> CREATE USER \'ecomuser\'@\'%\' IDENTIFIED BY\'ecompassword\';
MariaDB \> GRANT ALL PRIVILEGES ON \*.\* TO \'ecomuser\'@\'%\';
MariaDB \> FLUSH PRIVILEGES;
```

**Load Product Inventory Information to database**

We have downloaded Product Inventory Information db-load-script.sql in /opt directory. 
Load /opt/db-load-script.sql in mysql.

target server: db

Run 

```
sudo mysql \< /opt/db-load-script.sql
```

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

**Configure httpd**

Make the php page the default page for httpd.

Configure httpd for index.php

```
sudo sed -i \'s/index.html/index.php/g\' /etc/httpd/conf/httpd.conf
```

**Start httpd**

Start and enable the httpd service.

```
sudo systemctl start httpd; sudo systemctl enable httpd
```

**Download code**

Download code from the git command. Install git if it\'s not available on the web server.

GIT repo URL: https://github.com/kodekloudhub/learning-app-ecommerce.git

target directory: /var/www/html/

target server: web

```sudo yum install -y git; sudo git clone https://github.com/kodekloudhub/learning-app-ecommerce.git
/var/www/html/
```

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

**Deploy webapp**

Check the default apache content by clicking on the tab called E-commerce Application on the top right of the terminal.

Apache web server is listening on port 80 by default.
