FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    apache2-utils \
    libapache2-mod-fcgid \
    && rm -rf /var/lib/apt/lists/*

# Enable necessary Apache modules
RUN a2enmod proxy_fcgi setenvif rewrite proxy proxy_http ssl headers

# Copy SSL certificates
COPY certs/apache-selfsigned.crt /etc/apache2/ssl/apache-selfsigned.crt
COPY certs/apache-selfsigned.key /etc/apache2/ssl/apache-selfsigned.key

# From Motion Global
#COPY certs/mg-ssl/www.smartbuyglasses.com2048.crt /etc/apache2/ssl/mg-server.crt
#COPY certs/mg-ssl/mutiSSL2048.key /etc/apache2/ssl/mg-server.key

# Copy updated Virtual Host configuration
COPY vhost.conf /etc/apache2/sites-enabled/000-default.conf

CMD ["apachectl", "-D", "FOREGROUND"]
