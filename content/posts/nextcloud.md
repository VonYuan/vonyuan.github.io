+++
title = 'Installation of Next Cloud With LAMP'
date = 2023-09-23T14:33:32+08:00
draft = false
+++

Step 1: Download NextCloud on Ubuntu 20.04
Log into your Ubuntu 20.04 server. Then download the NextCloud zip archive onto your server. The latest stable version is 21.0.1 at the time of this writing. You may need to change the version number. Go to NextCloud's official website and click the download for server button to see the latest version.

bash
Copy code
wget https://download.nextcloud.com/server/releases/nextcloud-21.0.1.zip
Once downloaded, extract the archive with unzip.

bash
Copy code
sudo apt install unzip
sudo unzip nextcloud-21.0.1.zip -d /var/www/
The -d option specifies the target directory. NextCloud web files will be extracted to /var/www/nextcloud/. Then we need to change the owner of this directory to www-data so that the web server (Apache) can write to this directory.

bash
Copy code
sudo chown www-data:www-data /var/www/nextcloud/ -R
Step 2: Create a Database and User for Nextcloud in MariaDB Database Server
Log into MariaDB database server with the following command. Since MariaDB is now using the unix_socket plugin to authenticate user login, there’s no need to enter MariaDB root password. We just need to prefix the mysql command with sudo.

bash
Copy code
sudo mysql
Then create a database for Nextcloud. This tutorial names the database nextcloud. You can use whatever name you like.

sql
Copy code
create database nextcloud;
Create the database user. Again, you can use your preferred name for this user. Replace your-password with your preferred password.

sql
Copy code
create user nextclouduser@localhost identified by 'your-password';
Grant this user all privileges on the nextcloud database.

sql
Copy code
grant all privileges on nextcloud.* to nextclouduser@localhost identified by 'your-password';
Flush privileges and exit.

sql
Copy code
flush privileges;
exit;
Step 3: Create an Apache Virtual Host for Nextcloud
Create a nextcloud.conf file in /etc/apache2/sites-available/ directory, with a command-line text editor like Nano.

bash
Copy code
sudo nano /etc/apache2/sites-available/nextcloud.conf
Copy and paste the following text into the file. Replace nextcloud.example.com with your own preferred sub-domain. Don’t forget to create a DNS A record for this sub-domain in your DNS zone editor.

apache
Copy code
<VirtualHost *:80>
        DocumentRoot "/var/www/nextcloud"
        ServerName nextcloud.example.com

        ErrorLog ${APACHE_LOG_DIR}/nextcloud.error
        CustomLog ${APACHE_LOG_DIR}/nextcloud.access combined

        <Directory /var/www/nextcloud/>
            Require all granted
            Options FollowSymlinks MultiViews
            AllowOverride All

           <IfModule mod_dav.c>
               Dav off
           </IfModule>

        SetEnv HOME /var/www/nextcloud
        SetEnv HTTP_HOME /var/www/nextcloud
        Satisfy Any

       </Directory>

</VirtualHost>
Save and close the file (To save a file in Nano text editor, press Ctrl+O, then press Enter to confirm. To exit, press Ctrl+X.)

Then enable this virtual host.

bash
Copy code
sudo a2ensite nextcloud.conf
Run the following command to enable required Apache modules.

bash
Copy code
sudo a2enmod rewrite headers env dir mime setenvif ssl
Then test Apache configuration.

bash
Copy code
sudo apache2ctl -t
If the syntax is OK, reload Apache for the changes to take effect.

bash
Copy code
sudo systemctl restart apache2
Step 4: Install and Enable PHP Modules
Run the following commands to install PHP modules required or recommended by NextCloud.

bash
Copy code
sudo apt install imagemagick php-imagick libapache2-mod-php7.4 php7.4-common php7.4-mysql php7.4-fpm php7.4-gd php7.4-json php7.4-curl php7.4-zip php7.4-xml php7.4-mbstring php7.4-bz2 php7.4-intl php7.4-bcmath php7.4-gmp
Reload Apache to use these modules.

bash
Copy code
sudo systemctl reload apache2
Step 5: Enable HTTPS
Now you can access the Nextcloud web install wizard in your web browser by entering the domain name for your Nextcloud installation.

Copy code
nextcloud.example.com
If the web page can’t load, you probably need to open port 80 in the firewall.

bash
Copy code
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
And port 443 as well.

bash
Copy code
sudo iptables -I INPUT -p tcp --dport 443 -j ACCEPT
Before entering any sensitive information, we should enable secure HTTPS connection on Nextcloud. We can obtain a free TLS certificate from Let’s Encrypt. Install Let’s Encrypt client (certbot) from the Ubuntu 20.04 repository.

bash
Copy code
sudo apt install certbot python3-certbot-apache
Python3-certbot-apache is the Apache plugin. Next, run the following command to obtain a free TLS certificate using the Apache plugin.

bash
Copy code
sudo certbot --apache --agree-tos --redirect --staple-ocsp --email you@example.com -d nextcloud.example.com
Where:

--apache: Use the Apache authenticator and installer
--agree-tos: Agree to Let’s Encrypt terms of service
--redirect: Enforce HTTPS by adding 301 redirect.
--staple-ocsp: Enable OCSP Stapling.
--email: Email used for registration and recovery contact.
-d flag is followed by a list of domain names, separated by commas. You can add up to 100 domain names.
