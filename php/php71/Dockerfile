FROM php:7.1-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libmemcached-dev \
    zlib1g-dev \
    unzip \
    libpng-dev \
    libzip-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libicu-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy custom PHP-FPM configuration
COPY php-fpm.conf /usr/local/etc/php-fpm.d/custom.conf
    
# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd zip mysqli pdo_mysql intl

# Install Xdebug
RUN pecl install xdebug-2.9.8 && docker-php-ext-enable xdebug

# Copy Xdebug config
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html
CMD ["php-fpm"]
