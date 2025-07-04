FROM php:8.2-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libmemcached-dev \
    libzstd-dev \
    zlib1g-dev \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libicu-dev \
    libzip-dev \
    pkg-config \
    libssl-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy custom PHP-FPM configuration
COPY php-fpm.conf /usr/local/etc/php-fpm.d/custom.conf
    
# Install GD extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Install other PHP extensions (added mysqli)
RUN docker-php-ext-install zip intl mysqli pdo_mysql

# Install Memcached extension
RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev && pecl install memcached && docker-php-ext-enable memcached

# Install Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Install Xdebug and enable it
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_port=9003" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.log_level=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html
CMD ["php-fpm"]
