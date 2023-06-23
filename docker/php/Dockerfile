FROM composer:2 AS composer
FROM php:8.1.1-fpm-alpine

WORKDIR /var/www/dices/back

RUN apk update && apk add --no-cache curl git zip \
    && rm -rf /tmp/* /var/lib/apk/*

RUN docker-php-ext-install pdo mysqli pdo_mysql 

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN apk add --no-cache supervisor tzdata
ENV TZ Europe/Moscow

COPY /docker/php/php.ini /usr/local/etc/php/php.ini
COPY /docker/php/fpm-pool.conf /usr/local/etc/php-fpm.d/www.conf

COPY composer.json .
COPY composer.lock .


RUN composer install --prefer-dist --no-scripts

COPY . .
RUN php artisan view:clear
RUN echo "" > ./storage/logs/laravel.log
RUN chown -R www-data:www-data storage
RUN chmod -R 755 storage