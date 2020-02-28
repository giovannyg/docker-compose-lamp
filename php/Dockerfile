FROM php:7.3-rc-fpm-alpine
RUN apk update; \
    apk upgrade; \
    apk add --update nodejs npm

RUN apk add --no-cache libzip-dev \
    && docker-php-ext-configure zip --with-libzip=/usr/include \
    && docker-php-ext-install zip

RUN apk add --no-cache libxml2-dev && \
    docker-php-ext-install soap

RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql
RUN apk add freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev
RUN docker-php-ext-install -j$(nproc) iconv 
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 
RUN docker-php-ext-install -j$(nproc) gd
COPY php.ini /usr/local/etc/php/php.ini

RUN curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/ \
        && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer