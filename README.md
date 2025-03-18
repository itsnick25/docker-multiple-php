1. Build a Docker Container Image
   - Use this command to build: `docker-compose up -d`
   - Use this command to re-build when have a update: `docker-compose up --build -d`
   - Use this command to restart everything: `docker-compose down && docker-compose up -d`
   - Use this command to stop the docker: `docker-compose down`
     
2. Add new website
   - Add the VirtualHost in file: `apache/vhost.conf`
     Here's an example:
     ```
     <VirtualHost *:80>
        ServerName new.domain
        ServerAlias new.domain
        DocumentRoot "/var/www/html/new-domain"
        <Directory "/var/www/html/new-domain">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ## PHP 7.1
        #ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php71_fpm:9000/var/www/html/new-domain/$1
        ## PHP 7.4
        #ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php74_fpm:9000/var/www/html/new-domain/$1
        ## PHP 8.2
        ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php82_fpm:9000/var/www/html/new-domain/$1
    
        ErrorLog /var/log/apache2/new-domain_error.log
        CustomLog /var/log/apache2/new-domain_access.log combined

        ### Add redirect from http to https
        #RewriteEngine On
        #RewriteCond %{HTTPS} !=on
        #RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

        # Set apache environment variables
        SetEnv OM_ENV development
     
     </VirtualHost>

     <VirtualHost *:443>
        ServerName new.domain
        ServerAlias new.domain
        DocumentRoot "/var/www/html/new-domain"
        <Directory "/var/www/html/new-domain">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ## PHP 7.1
        #ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php71_fpm:9000/var/www/html/new-domain/$1
        ## PHP 7.4
        #ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php74_fpm:9000/var/www/html/new-domain/$1
        ## PHP 8.2
        ProxyPassMatch "^/(.*\.php(/.*)?)$" fcgi://php82_fpm:9000/var/www/html/new-domain/$1
    
        SSLEngine on
        SSLCertificateFile /etc/apache2/ssl/mg-ssl/www.smartbuyglasses.com2048.crt
        SSLCertificateKeyFile /etc/apache2/ssl/mg-ssl/mutiSSL2048.key
    
        ErrorLog /var/log/apache2/om_error.log
        CustomLog /var/log/apache2/om_access.log combined
    
        # Set apache environment variables
        SetEnv OM_ENV development
     </VirtualHost>

    
   - Code structure: 
      * CMS: /www/html/cms
      * OM: /www/html/om2012
      * SBG Zend: /www/html/sbg-desktop
      * SBG Laravel: /www/html/sbg-laravel
      * Middle Layer: /www/html/sbg-middlelayer
      * Localhost: /www/html/local
      
   - Run this command to re-build docker: `docker-compose up --build -d`
   - Run this command to create a custom_network manually: `docker network create --driver bridge --subnet 192.168.100.0/24 custom_network`
   - Open new site `new.domain` on browser
