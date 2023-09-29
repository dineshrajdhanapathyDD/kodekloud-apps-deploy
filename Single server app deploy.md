# Introduction

This is a sample e-commerce application built process. 

Let us now setup the complete app - both web and database - on a single
server. Install mariadb and start the process with default conf. Ensure
that process is started on server reboot.

## Deploy and Configure Database

**Install MariaDB**

```
sudo yum install -y mariadb-server
sudo systemctl start mariadb
sudo service mariadb start
sudo systemctl enable mariadb
```

![b1](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/7d920da7-daf9-4b4c-8b3b-e0b5a1c68300)

![b2](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/e4c3acc7-b97f-4e76-aaea-5d9714b0bd04)

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

![b3](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/78de451c-86e4-4985-a903-d711e46f31ea)

![b4](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/b94c4214-8259-4911-bdcf-bd12fded5365)

**Load Product Inventory Information to database**

We have downloaded Product Inventory Information db-load-script.sql
in /opt directory. Load /opt/db-load-script.sql in mysql

Run 
```
sudo mysql \< /opt/db-load-script.sql
```

![b5](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/8ea2ce13-8417-4a23-9edc-e788c3411a56)

## Deploy and Configure Web

**Install below packages**

httpd

php

php-mysqlnd

```
sudo yum install -y httpd php php-mysqlnd
```

![b6](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/40b13a75-51b1-4be7-a43f-4284db89139f)

![b7](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/7576cc1d-f5f0-4316-bffd-fc0d530e2c67)

**Configure httpd**

Make the php page the default page for httpd.

Update the httpd.conf file to use index.php instead of index.html

```
sudo sed -i \'s/index.html/index.php/g\'
/etc/httpd/conf/httpd.conf
```

![b8](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/7bdd58a5-128f-4011-b4b3-768eae03bd6d)

**Start httpd**

Start and enable httpd

```
sudo systemctl start httpd ; sudo systemctl enable httpd
```

![b9](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/c506bfb2-97c8-431f-9539-40ca1e5449c7)

**Download code**

Download code with git. Install git if necessary

GIT repo URL : https://github.com/kodekloudhub/learning-app-ecommerce.git

target directory : /var/www/html/

```
sudo yum install -y git; sudo git clone
https://github.com/kodekloudhub/learning-app-ecommerce.git
/var/www/html/
```

![b10](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/98df47f6-3273-447b-bab1-12939936102e)

**Update index.php**

Since we have both the web and database on the same server, update
the IP address configured in index.php file to localhost.

Change 172.20.1.101 to localhost in index.php file.

aUse VI editor to edit the index.php file OR run the command 

```
sudo sed -i \'s/172.20.1.101/localhost/g\' /var/www/html/index.php
``` 
to update IP.

![b11](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/6a8badf1-f492-41c5-8878-4e82e88bc7f1)

**Test**

Please check default apache content by clicking tab called ecommerce
Application on the top right of the terminal.

Apache is listening on port 80 by default.

check the port for the deploy apps.

![b12](https://github.com/dineshrajdhanapathyDD/kodekloud-apps-deploy/assets/52989362/93ac96f5-1744-40e2-a209-48851af0df16)























