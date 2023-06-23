FROM composer:2 AS composer
FROM php:8.1.1-fpm-alpine

WORKDIR /var/www/dices/back

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN apk update && apk add --no-cache curl git zip \
    g++ \
    make \
    autoconf \
    && rm -rf /tmp/* /var/lib/apk/*

# RUN docker-php-ext-configure zip \
#     && docker-php-ext-configure pdo_mysql \
#     && docker-php-ext-install -j$(nproc) zip \
#     && docker-php-ext-install -j$(nproc) pdo_mysql

# docker-php-ext-configure pdo_mysql zip \
    # && docker-php-ext-install -j$(nproc) pdo mysqli pdo_mysql zip


RUN set -xe \
    && apk add --no-cache \
    $PHPIZE_DEPS \
    ffmpeg \
    libavif-dev \
    libwebp-dev \
    libzip-dev \
    libwebp-tools \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    gmp-dev \
    mysql-client \
    && docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg \
    --with-webp \
    && docker-php-ext-configure zip \
    && docker-php-ext-configure gmp \
    && docker-php-ext-configure pdo_mysql \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) exif \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) gmp \
    && pecl install pcov \
    vim

RUN apk add --no-cache supervisor tzdata
ENV TZ Europe/Moscow

COPY /images/php/php.ini /usr/local/etc/php/php.ini
COPY /images/php/fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf

COPY composer.json .
COPY composer.lock .


RUN composer install --prefer-dist --no-scripts

COPY . .
RUN php artisan view:clear
RUN echo "" > ./storage/logs/laravel.log
RUN chown -R www-data:www-data storage
RUN chmod -R 755 storage