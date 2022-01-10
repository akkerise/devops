FROM php:7.3-fpm-alpine

# Copy composer.lock and composer.json
#COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www/public_html

RUN apk add mysql-client msmtp perl wget procps shadow libzip libpng libjpeg-turbo libwebp freetype icu autoconf bash openssl

RUN apk update && apk add libxml2-dev libxslt-dev

RUN apk add --virtual build-essentials \
    icu-dev icu-libs zlib-dev g++ make automake autoconf shadow libzip-dev \
    libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev && \
    docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ && \
    docker-php-ext-configure intl && \
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

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
#COPY ../ /var/www
COPY ./php-fpm/* /usr/local/etc/php-fpm.d/
COPY ./php/* /usr/local/etc/php/conf.d/

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
#EXPOSE 9000

#CMD ["php-fpm"]
