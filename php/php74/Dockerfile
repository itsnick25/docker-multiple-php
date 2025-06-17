FROM php:7.4-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libmemcached-dev \
    libzstd-dev \
    zlib1g-dev \
    unzip \
    curl \
    libpng-dev \
    libzip-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libicu-dev \
    pkg-config \
    libssl-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy custom PHP-FPM configuration
COPY php-fpm.conf /usr/local/etc/php-fpm.d/custom.conf
    
# Update PECL channel
RUN pecl channel-update pecl.php.net

# Install Xdebug and enable it
RUN pecl install xdebug-3.1.6 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode = debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request = yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host = host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_port = 9003" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.log_level=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install Memcached (fix for missing dependencies)
RUN pecl install memcached && docker-php-ext-enable memcached

# Install Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Configure GD before installation
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo_mysql zip gd intl

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html
CMD ["php-fpm"]
