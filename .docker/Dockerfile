ARG PHP_VERSION=8.1

FROM php:${PHP_VERSION}-fpm

ARG UID=1000
ARG UNAME=marvin
ARG WORKDIR=/var/www

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Copy local php configuration into image
COPY .docker/php/php-custom.ini $PHP_INI_DIR/conf.d/php.ini
COPY .docker/php/xdebug.ini $PHP_INI_DIR/conf.d/xdebug.ini

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

RUN docker-php-ext-enable opcache gd zip pdo_mysql intl iconv bcmath

# Create user $UNAME with $UID to match host users id to prevent permission issues for created files
RUN useradd -G www-data,root -u $UID -d /home/$UNAME $UNAME
RUN mkdir -p /home/$UNAME/.composer && chown -R $UNAME:$UNAME /home/$UNAME


# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer



COPY composer.json composer.lock ./


RUN set -eux; \
    composer install --ignore-platform-reqs --prefer-dist --no-autoloader --no-scripts  --no-progress; \
	composer clear-cache

RUN set -eux; \
    composer dump-autoload --classmap-authoritative

# Switch workin directory
WORKDIR ${WORKDIR}

USER $UNAME

#RUN usermod -u 1000 www-data
