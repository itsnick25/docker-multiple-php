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
RUN pecl install memcached && docker-php-ext-enable memcached

# Install Xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Copy Xdebug config
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www/html
CMD ["php-fpm"]
