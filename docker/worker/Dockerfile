FROM php:7.4-fpm-alpine

RUN apk add mysql-client msmtp perl wget procps shadow libzip libpng libjpeg-turbo libwebp freetype icu autoconf supervisor bash openssl

RUN apk update && apk add libxml2-dev libxslt-dev

RUN apk add --virtual build-essentials \
    icu-dev icu-libs zlib-dev g++ make automake autoconf shadow libzip-dev \
    libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev && \
    docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp && \
    docker-php-ext-install gd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install intl && \
    docker-php-ext-install opcache && \
    docker-php-ext-install exif && \
    docker-php-ext-install zip && \
    docker-php-ext-install pcntl && \
    docker-php-ext-install json && \
    docker-php-ext-install tokenizer && \
    docker-php-ext-install -j$(nproc) dom xml && \
    apk del build-essentials && rm -rf /usr/src/php*


RUN apk update \
    && apk --no-cache --update add build-base \
    && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis

COPY ./php-fpm/* /usr/local/etc/php-fpm.d/
COPY ./php/* /usr/local/etc/php/conf.d/

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]
