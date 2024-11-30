FROM php:8.2-fpm-alpine

COPY ./docker/php/php.ini /usr/local/etc/php/php.ini
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN apk --update --no-cache add \
    curl \
    libzip-dev \
    libpng-dev \
    autoconf \
    gcc \
    g++ \
    make \
    git \
    bash \
    icu-dev \
    oniguruma-dev \
    nodejs \
    npm \
    patch \
    linux-headers

RUN docker-php-ext-install \
    pdo_mysql \
    bcmath \
    mbstring \
    zip \
    opcache \
    gd \
    intl

# Install the latest version of xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

WORKDIR /var/www/app

