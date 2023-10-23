FROM php:${PHP_VERSION:-8.1}-fpm

ARG UID=1000
ARG UNAME=marvin
ARG WORKDIR=/var/www

RUN usermod -u $UID www-data
RUN groupmod -g $UID www-data

# Use php.ini-development as default init
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Install system packages
RUN apt-get update && apt-get install -y apt-transport-https && \
    # Install system libraries
    apt-get install -y \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    git \
    curl \
    zip \
    unzip \
    libicu-dev \
    acl && \
    # Cleanup
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    pecl install xdebug


# Install php extensions
RUN docker-php-ext-install \
    opcache \
    gd \
    zip \
    pdo_mysql \
    intl \
    iconv \
    bcmath

# Configure GD
RUN docker-php-ext-configure gd --with-jpeg

# Enable extensions
RUN docker-php-ext-enable xdebug opcache gd zip pdo_mysql intl iconv bcmath

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY composer.json composer.lock ./

RUN set -eux; \
    composer install --ignore-platform-reqs --prefer-dist --no-autoloader --no-scripts  --no-progress; \
	composer clear-cache

RUN set -eux; \
    composer dump-autoload --classmap-authoritative

# Switch working directory
WORKDIR ${WORKDIR}

#RUN usermod -u 1000 www-data
