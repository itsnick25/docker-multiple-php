FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    apache2-utils \
    libapache2-mod-fcgid \
    dnsutils \
    iputils-ping \
    && rm -rf /var/lib/apt/lists/*

# Enable necessary Apache modules
RUN a2enmod proxy_fcgi setenvif rewrite proxy proxy_http ssl headers

# Copy SSL certificates
COPY certs/apache-selfsigned.crt /etc/apache2/ssl/apache-selfsigned.crt
COPY certs/apache-selfsigned.key /etc/apache2/ssl/apache-selfsigned.key

# Copy updated Virtual Host configuration
COPY vhost.conf /etc/apache2/sites-enabled/000-default.conf

#RUN mkdir -p /var/www/html/session && chmod -R 777 /var/www/html/session
#RUN mkdir -p /var/www/html/session/mobile && chmod -R 777 /var/www/html/session/mobile
#RUN chmod -R 777 /var/www/html/sbg-desktop/public/session/sbg2012
#RUN chmod -R 777 /var/www/html/sbg-desktop/public/session/mobile

EXPOSE 80

# For HTTPS
EXPOSE 443

CMD ["apachectl", "-D", "FOREGROUND"]