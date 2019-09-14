## Vagrant enviroment

	- Download and setup
		+ VirtualBox: https://www.virtualbox.org/wiki/Downloads
		+ Vagrant: https://www.vagrantup.com/downloads.html
	- Enable Virtualization Technology in BIOS
		+ Security -> System security -> Virtualization Technology -> Enable
		+ Press F10 and restart device

## Vagrant

	- Create box
		+ vagrant init centos/7
		+ vagrant up
	- Copy code to VagrantFile:
		# -*- mode: ruby -*-
		# vi: set ft=ruby :

		Vagrant.configure("2") do |config|
		  config.vm.box = "centos/7"
		  config.vm.network :private_network, ip: "192.168.33.50"
		  config.vm.synced_folder "C:/WS", "/var/www/html", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777", "fmode=777"]
		end

## EPEL(install phpMyAdmin)

	- rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY*
	- yum -y install epel-release

## MySQL/MariaDB

	- Install:
		+ yum -y install mariadb-server mariadb
		+ systemctl start mariadb.service
		+ systemctl enable mariadb.service
	- Set passwords for the MySQL root account:
		+ mysql_secure_installation

## APACHE

	- Install:
		+ yum -y install httpd
	- Configure system to start Apache at boot time
		+ systemctl start httpd.service
		+ systemctl enable httpd.service
	- Disable selinux
		+ vi /etc/sysconfig/selinux
		+ Change SELINUX=enforcing => SELINUX=disabled

## PHP

	- Add Remi CentOS repository: rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm
	- Install yum-utils: yum -y install yum-utils
	- Update yum: yum update
	- Install PHP 7.2:
		+ yum-config-manager --enable remi-php72
		+ yum -y install php php-opcache
	- Restart apache: systemctl restart httpd.service
	- Install extension: sudo yum install php-{package_name}

## phpMyAdmin

	- Install: yum -y install phpMyAdmin
	- Configure phpMyAdmin:
		+ vi /etc/httpd/conf.d/phpMyAdmin.conf
		+ Edit and save:
			[...]
			Alias /phpMyAdmin /usr/share/phpMyAdmin
			Alias /phpmyadmin /usr/share/phpMyAdmin

			<Directory /usr/share/phpMyAdmin/>
			AddDefaultCharset UTF-8

			<IfModule mod_authz_core.c>
				# Apache 2.4
				# <RequireAny>
				# Require ip 127.0.0.1
				# Require ip ::1
				# </RequireAny>
				Require all granted
				</IfModule>
				<IfModule !mod_authz_core.c>
				# Apache 2.2
				Order Deny,Allow
				Deny from All
				Allow from 127.0.0.1
				Allow from ::1
				</IfModule>
			</Directory>
			<Directory /usr/share/phpMyAdmin/>
			        Options none
			        AllowOverride Limit
			        Require all granted
			</Directory>

			[...] 
	- Restart Apache: systemctl restart  httpd.service

## VIRTUAL HOST

	Create virtual host:
		- Create config file: vi /etc/httpd/conf.d/{hostname}.conf
		- Add code
			<VirtualHost *:80>
			  ServerName www.{hostname}
			  ServerAlias {hostname}
			  DocumentRoot /var/www/html/{folder_name}
			  <Directory /var/www/html/{folder_name}/>
			    Options FollowSymLinks
			    AllowOverride All
			  </Directory>
			</VirtualHost>
		- Restart Apache: systemctl restart httpd.service
	Create virtual host + SSL:
		- Create config file: vi /etc/httpd/conf.d/{hostname}.conf
		- Add code
			<VirtualHost *:443>
			  ServerName www.{hostname}
			  ServerAlias {hostname}
			  DocumentRoot /var/www/html/{folder_name}
			  <Directory /var/www/html/{folder_name}/>
			    Options FollowSymLinks
			    AllowOverride All
			  </Directory>
			  SSLEngine on
			  SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
			  SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
			</VirtualHost>
		- Install Mod SSL: yum install mod_ssl
		- Create new certificate:
			+ mkdir /etc/ssl/private
			+ chmod 700 /etc/ssl/private
			+ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
		- Restart Apache: systemctl restart httpd.service

## TMUX

	- Install: yum install tmux
	- Create section: tmux new -s share
	- Split vertical: Ctrl + b and Shift + "
	- Split horizontal: Ctrl + b and Shift + 5

## GIT 

	- Instal: yum install git
	- Add config
		+ git config --global user.name "Your Name"
		+ git config --global user.email "you@example.com"

	Add ssh key:
	- cd ~/.ssh
	- ssh-keygen
	- Enter password.
	- vi ~/.ssh/id_rsa.pub
	- Copy key and add this key into git profile.
