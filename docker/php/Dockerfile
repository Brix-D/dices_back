FROM composer:2 AS composer
FROM php:8.1.1-fpm-alpine

WORKDIR /var/www/dices/back

RUN apk update && apk add --no-cache curl git zip \
    && rm -rf /tmp/* /var/lib/apk/*

RUN docker-php-ext-install pdo mysqli pdo_mysql 


COPY --from=composer /usr/bin/composer /usr/bin/composer

