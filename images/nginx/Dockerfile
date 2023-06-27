FROM nginx:alpine

RUN rm /etc/nginx/conf.d/*

WORKDIR /var/www/dices/back

COPY ./images/nginx/nginx.conf /etc/nginx/
COPY ./images/nginx/dices.ru.conf /etc/nginx/conf.d/

COPY ./public /var/www/dices/back/public
