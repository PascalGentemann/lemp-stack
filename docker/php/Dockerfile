FROM php:7.1.14-fpm-alpine

# persistent / runtime deps
ENV PHPIZE_DEPS \
    autoconf \
    cmake \
    file \
    g++ \
    gcc \
    libc-dev \
    pcre-dev \
    make \
    git \
    pkgconf \
    re2c

RUN apk add --no-cache --virtual .persistent-deps \
    tzdata \
    git \
    icu-dev \
    nodejs \
    file \
    libtool \
    libmcrypt-dev \
    zlib-dev \
    libpng-dev \
    libxml2-dev

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        zeromq-dev \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-configure bcmath --enable-bcmath \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure pcntl --enable-pcntl \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql \
    && docker-php-ext-configure mbstring --enable-mbstring \
    && docker-php-ext-install \
        opcache \
        bcmath \
        intl \
        pcntl \
        pdo_mysql \
        mbstring \
        mysqli \
        mcrypt \
        zip \
        gd \
        soap \
    && apk del .build-deps \
    && rm -rf /tmp/* \
    && rm -rf /var/www \
    && mkdir -p /var/www

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Enable and configure xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-2.5.0 \
    && docker-php-ext-enable xdebug

# Copy configuration
COPY config/php.ini /usr/local/etc/php/
COPY config/fpm/php-fpm.conf /usr/local/etc/
COPY config/fpm/pool.d /usr/local/etc/pool.d

# Create Log Folder
RUN mkdir -p /var/log/php

RUN apk update && apk add nano

WORKDIR /var/www
