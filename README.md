# README
https://websiteforstudents.com/install-magento-2-using-composer-on-ubuntu-16-04-18-04-with-apache2-mariadb-and-php-7-1-support/



//------

Skip to content

Website for Students

Learn Ubuntu Linux, Windows and CMS

    Articles
    Ubuntu
    CMS Directory
    About Us

Install Magento 2 using Composer on Ubuntu 16.04 | 18.04 with Apache2, MariaDB and PHP 7.1 Support
Posted on 09/07/2018 by Student	

If you always want to upgrade Magento 2 to the latest version easily, the steps below should help you get there… This post helps you setup Magento 2 with Composer for easy install and upgrade…

Students and new users looking for help installing the latest version of Magento 2 ( 2.2.5 ) from Github using Composer with Apache2, MariaDB and PHP 7.2 support, the steps below should be a great place to start…

When you use Composer to install Magento 2 packages, you can easily upgrade from the commmand line with Composer, which is much simpler…

To upgrade Magento 2, you must manually upgrade its core files and other packages when new versions are available…. and doing that using its starndard method can be challenging for some users…

This brief tutorial is going to show students and new users how to install / upgrade Magento 2 from Github repository via Composer with Apache2, MariaDB and PHP 7.2 support on Ubuntu 16.04 | 18.04 LTS servers…

To get started with installing Magento 2, follow the steps below:
Step 1: Install Apache2 HTTP Server on Ubuntu

Apache2 HTTP Server is the most popular web server in use… so install it since Magento 2 needs it..

To install Apache2 HTTP on Ubuntu server, run the commands below…

sudo apt update
sudo apt install apache2

After installing Apache2, the commands below can be used to stop, start and enable Apache2 service to always start up with the server boots.

sudo systemctl stop apache2.service
sudo systemctl start apache2.service
sudo systemctl enable apache2.service

To test Apache2 setup, open your browser and browse to the server hostname or IP address and you should see Apache2 default test page as shown below.. When you see that, then Apache2 is working as expected..
http://localhost
apache2 ubuntu install
Step 2: Install MariaDB Database Server

MariaDB database server is a great place to start when looking at open source database servers to use with Magento… To install MariaDB run the commands below…

sudo apt-get install mariadb-server mariadb-client

After installing MariaDB, the commands below can be used to stop, start and enable MariaDB service to always start up when the server boots..

Run these on Ubuntu 16.04 LTS

sudo systemctl stop mysql.service
sudo systemctl start mysql.service
sudo systemctl enable mysql.service

Run these on Ubuntu 18.04 and 18.10 LTS

sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service

After that, run the commands below to secure MariaDB server by creating a root password and disallowing remote root access.

sudo mysql_secure_installation

When prompted, answer the questions below by following the guide.

    Enter current password for root (enter for none): Just press the Enter
    Set root password? [Y/n]: Y
    New password: Enter password
    Re-enter new password: Repeat password
    Remove anonymous users? [Y/n]: Y
    Disallow root login remotely? [Y/n]: Y
    Remove test database and access to it? [Y/n]:  Y
    Reload privilege tables now? [Y/n]:  Y

Restart MariaDB server

To test if MariaDB is installed, type the commands below to logon to MariaDB server

sudo mysql -u root -p

Then type the password you created above to sign on… if successful, you should see MariaDB welcome message
mariadb welcome
Step 3: Install PHP 7.1 and Related Modules

PHP 7.1 may not be available in Ubuntu default repositories… in order to install it, you will have to get it from third-party repositories.

Run the commands below to add the below third party repository to upgrade to PHP 7.1

sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php

Then update and upgrade to PHP 7.1

sudo apt update

Next, run the commands below to install PHP 7.1 and related modules.

sudo apt install php7.1 libapache2-mod-php7.1 php7.1-common php7.1-gmp php7.1-curl php7.1-soap php7.1-bcmath php7.1-intl php7.1-mbstring php7.1-xmlrpc php7.1-mcrypt php7.1-mysql php7.1-gd php7.1-xml php7.1-cli php7.1-zip

After installing PHP 7.1, run the commands below to open PHP default config file for Apache2…

sudo nano /etc/php/7.1/apache2/php.ini

Then make the changes on the following lines below in the file and save. The value below are great settings to apply in your environments.

file_uploads = On
allow_url_fopen = On
short_open_tag = On
memory_limit = 256M
upload_max_filesize = 100M
max_execution_time = 360
date.timezone = America/Chicago

After making the change above, save the file and close out.
Step 3: Restart Apache2

After installing PHP and related modules, all you have to do is restart Apache2 to reload PHP configurations…

To restart Apache2, run the commands below

sudo systemctl restart apache2.service

To test PHP 7.1 settings with Apache2, create a phpinfo.php file in Apache2 root directory by running the commands below

sudo nano /var/www/html/phpinfo.php

Then type the content below and save the file.

<?php phpinfo( ); ?>

Save the file.. then browse to your server hostname followed by /phpinfo.php
http://localhost/phpinfo.php

You should see PHP default test page…
PHP 7.2 ubuntu nginx
Step 4: Create Magento 2 Database

Now that you’ve installed all the packages that are required for Magento 2 to function, continue below to start configuring the servers. First run the commands below to create a blank Magento 2 database.

To logon to MariaDB database server, run the commands below.

sudo mysql -u root -p

Then create a database called magento2

CREATE DATABASE magento2

Create a database user called magento2user with new password

CREATE USER 'magento2user'@'localhost' IDENTIFIED BY 'new_password_here';

Then grant the user full access to the database.

GRANT ALL ON magento2.* TO 'magento2user'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;

Finally, save your changes and exit.

FLUSH PRIVILEGES;
EXIT;

Step 5: Download Magento 2 Latest Release

To get Magento 2 latest release you may want to use Github repository… Install Composer, Curl and other dependencies to get started…

sudo apt install curl git
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

After installing curl and Composer above, change into the Apache2 root directory and downaload Magento 2 packages from Github…

When prompted, enter your authentication keys. Your public key is your username; your private key is your password….  ( https://marketplace.magento.com/customer/accessKeys/ )

You’ll have to register for an account to create the key above….

cd /var/www/html
sudo composer create-project --repository=https://repo.magento.com/ magento/project-community-edition magento2

Then run the commands below to install Magento 2 with the following options:

    The Magento software is installed in the root directory on localhost…. Admin is admin;  therefore: Your storefront URL is http://exmaple.com
    The database server is on the same localhost as the webserver….
    The database name is magento2, and the magento2user and password is new_passwore_here
    Uses server rewrites
    The Magento administrator has the following properties:
        First and last name are: Admin User
        Username is: admin
     and the password is admin123
    E-mail address is: admin@example.com
    Default language is: (U.S. English)

    Default currency is: U.S. dollars
    Default time zone is: U.S. Central (America/Chicago)

The commands below:

cd /var/www/html/magento2
sudo bin/magento setup:install --base-url=http://example.com/ --db-host=localhost --db-name=magento2 --db-user=magento2user --db-password=new_password_here --admin-firstname=Admin --admin-lastname=User --admin-email=admin@example.com --admin-user=admin --admin-password=admin123 --language=en_US --currency=USD --timezone=America/Chicago --use-rewrites=1

Then run the commands below to set the correct permissions for Magento 2 to function.

sudo chown -R www-data:www-data /var/www/html/magento2/
sudo chmod -R 755 /var/www/html/magento2/

Step 6: Configure Apache2

Finally, configure Apahce2 site configuration file for Magento 2. This file will control how users access Magento 2 content. Run the commands below to create a new configuration file called magento2.conf

sudo nano /etc/apache2/sites-available/magento2.conf

Then copy and paste the content below into the file and save it. Replace the highlighted line with your own domain name and directory root location.

<VirtualHost *:80>
     ServerAdmin admin@example.com
     DocumentRoot /var/www/html/magento2/
     ServerName example.com
     ServerAlias www.example.com

     <Directory /var/www/html/magento2/>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Save the file and exit.
Step 7: Enable the Magento 2 and Rewrite Module

After configuring the VirtualHost above, enable it by running the commands below

sudo a2ensite magento2.conf
sudo a2enmod rewrite

Step 8 : Restart Apache2

To load all the settings above, restart Apache2 by running the commands below.

sudo systemctl restart apache2.service

Then open your browser and browse to the server domain name. You should see Magento 2 setup wizard to complete. Please follow the wizard carefully.
http://example.com/
Magento Ubuntu Install

Congratulation! You have successfully installed Magento 2 on Ubuntu 16.04 | 18.04 and may work on upcoming 18.10…

In the future when you want to upgrade to a new released version, simply run the commands below to upgrade…

cd /var/www/html/magento2
sudo bin/magento maintenance:enable
sudo composer require magento/product-community-edition 2.2.5 --no-update
sudo composer update
sudo php bin/magento setup:upgrade
sudo php bin/magento setup:di:compile
sudo php bin/magento indexer:reindex
sudo php bin/magento maintenance:disable

You may have to re-run the to update Apache2 directory permissions…

That’s it!

You may also like the post below:
Posted in Applications, Labs, Linux UbuntuTagged Magento2, Ubuntu 16.04 LTS, Ubuntu 18.04	
Post navigation
← Previous Post
Next Post →
10 thoughts on “Install Magento 2 using Composer on Ubuntu 16.04 | 18.04 with Apache2, MariaDB and PHP 7.1 Support”

    Don says:	
    10/12/2018 at 3:34 PM

    I can not figure out how to download the code from github and run the composer install, both require the folder to be empty:
    if i do this first:
    “git clone https://github.com/magento/magento2.git”
    then when i try:
    “sudo composer create-project –repository=https://repo.magento.com/ magento/project-community-edition magento2”
    it says folder must be empty
    And vice versa.
    Don
    Reply
        Aran Ariel says:	
        11/17/2018 at 12:21 PM

        You can download on Magento’s site and unzip the files into var/www/html/magento2
        You then need as follows:
        cd /var/www/html
        sudo composer create-project –repository=https://repo.magento.com/ magento/project-community-edition magento2
        Reply
    Aran Ariel says:	
    11/17/2018 at 12:25 PM

    There is a minor typo throughout the document relating to php 7.2, however the instructions are for 7.1 which is alright.
    PHP. P HP 7.0.13–7.0.x and PHP 7.1.x are the only versions currently supported by Magento.
    Reply
    Sneha says:	
    01/13/2019 at 4:35 AM

    After installing magento2.3, all css and js file missing. Can anyone help me to resolve this issue ?
    Reply
        Cristi Enache says:	
        03/08/2019 at 11:00 AM

        sudo nano /etc/apache2/apache2.conf — set AllowOverride All
        Reply
        mike says:	
        02/17/2020 at 6:11 AM

        sudo bin/magento setup:static-content:deploy (language iso code) -f (if you are under developement or default stages)

        in other words i would use it like this:

        sudo bin/magento setup:static-content:deploy es_MX -f

        es_MX stands for spanish_MeXico
        and -f to force the deployment
        Reply
    patryk says:	
    04/14/2019 at 11:18 AM

    all went pretty smooth, but when opening http://example.com/ i’m getting generic website rather than magento? any ideas what went wrong? thanks
    Reply
        Student says:	
        04/14/2019 at 12:44 PM

        You’ll need to replace example.com with your domain or add example.com to your /etc/hosts file…
        Reply
            Patryk says:	
            04/17/2019 at 3:21 PM

            Thanks for reply.
            Hmmm …. It wasn’t that. I had to reboot Ubuntu.
            Started mariadb and apache2 and seems to be working (for now).
            Playing with apache settings will throw it out back to default apache page and necessity of full reboot a again.
            Any ideas please?
            Reply
    Yuvraj says:	
    02/16/2020 at 5:25 PM

    Hi, I didn’t get admin panel access for backend dashboard. How to get access or any link to magento backend dashboard?
    Reply

Leave a Reply

Your email address will not be published. Required fields are marked *

Comment

Name *

Email *

This site uses Akismet to reduce spam. Learn how your comment data is processed.



Currently Trending

    Setup Let’s Encrypt Wildcard SSL on Ubuntu 20.04 | 18.04
    How to Install Go on Ubuntu 20.04 | 18.04
    How to Configure Timezone on Ubuntu 20.04 | 18.04
    How to Install Yarn on Ubuntu 20.04 | 18.04
    Install phpMyAdmin on Ubuntu 20.04 | 18.04 with Apache
    How to take Screenshots on Ubuntu 20.04 | 18.04
    Install Samba on Ubuntu 20.04 | 18.04
    How to Install Python pip on Ubuntu 20.04 | 18.04







