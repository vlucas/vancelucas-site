---
title: 'Setup Ubuntu 12.04 with Nginx and PHP-FPM'
date: 2011-12-08
tags: nginx, php, php, php-fpm, servers, ubuntu
published: false
---

###If you are getting the error:


>Couldn't find any package whose name or description matched "php5-fpm"

The instructions below are how to solve it and get the most up-to-date version of PHP and PHP-FPM installed on your Ubuntu box or VPS.

###Update your Package Lists


>sudo apt-get update
sudo apt-get upgrade


###Remove Apache2 if present

If apache2 is currently installed and you do not want it to be, run the commands below to totally uninstall and remove it from Ubuntu.
**Note that all your Apache2 vhosts and configuration will be deleted**
, so download them or back them up if you need them.

>sudo apt-get -y remove --purge apache2 apache2-utils
sudo rm -rf /etc/apache2


###Install Aptitude


>sudo apt-get -y install aptitude
sudo aptitude -y update


###Install Nginx


>sudo aptitude -y install nginx
sudo /etc/init.d/nginx start


###Add Nginx PHP source

There is no 'php5-fpm' package in the default repository list that ships with Ubuntu, so you will have to add the
[Debian PPA](https://launchpad.net/~ondrej/+archive/php5).

>sudo add-apt-repository -y ppa:ondrej/php5
sudo aptitude update

If you get an error that says 'add-apt-repository: command not found', you have to install the 'python-software-properties' package first:

>sudo aptitude -y install python-software-properties


###Install PHP and PHP-FPM

Now we can install PHP with PHP-FPM

>sudo aptitude -y install php5-cli php5-common php5-mcrypt php5-suhosin php5-gd php5-fpm php5-cgi php-pear php5-memcache php-apc php5-curl


###Optionally Install PDO and MySQL


>sudo apt-get -y install apt-get install mysql-client mysql-server libmysqlclient15-dev
sudo aptitude -y install php5-mysql


###Edit your PHP-FPM config

Now open your PHP-FPM nginx config file using a server editor like vim (or "vi" if vim is not installed):

>vim /etc/php5/fpm/pool.d/www.conf

Look for the key named 
**pm.max_children**
. By default this is set to 50, but if you are running a small VPS, you will need to bring this down to 10 or so. Also make sure to check to values of 
**pm.start_servers**
, 
**pm.min_spare_servers**
, and 
**pm.max_spare_servers**
 to ensure they are within the max_children limit you set or nginx will not start.

While you are in there, also consider uncommenting 
**pm.max_requests**
 to ensure your memory usage stays low, especially if you are working with an older codebase, image or video processing, or a lot of 3rd party libraries.

###Configure Nginx

Be sure to read up on 
[nginx and PHP-FPM security](https://nealpoole.com/blog/2011/04/setting-up-php-fastcgi-and-nginx-dont-trust-the-tutorials-check-your-configuration/) before you do anything else or begin setting up your websites.

###Start nginx


>sudo service php5-fpm start


###<<<Still Need>>>


*Install MySQL / other db


*Install cURL / other dependencies


*Restart PHP-FPM after any changes

###Optimizing for Performance

Great guide for small VPSes: http://www.earnersblog.com/vps-optimization-guide/
