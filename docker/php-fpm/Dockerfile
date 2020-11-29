# base
FROM php:7.4-fpm-alpine AS php_fpm_base

RUN apk update && apk add --no-cache libjpeg-turbo libpng libwebp libjpeg-turbo-dev libpng-dev libwebp-dev curl-dev icu-dev libzip-dev

RUN apk add --no-cache $PHPIZE_DEPS && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure gd \
      --with-jpeg=/usr/include/ \
      --with-webp=/usr/include/ \
      --enable-gd && \
    docker-php-ext-install bcmath opcache gd pdo_mysql exif intl zip sockets && \
    pecl install redis && docker-php-ext-enable redis && \
    cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# dev
FROM php_fpm_base AS php_fpm_develop

RUN apk add --no-cache $PHPIZE_DEPS && \
    pecl install xdebug-2.9.1 && \
    docker-php-ext-enable xdebug && \
    cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# cron
FROM php_fpm_base AS php_fpm_cron

COPY crontab/root /etc/crontabs/root
RUN chown root:root /etc/crontabs/root && \
    chmod 0600 /etc/crontabs/root

