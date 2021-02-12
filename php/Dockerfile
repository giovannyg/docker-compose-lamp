FROM php:7.4-rc-fpm-alpine

WORKDIR /usr/local/apache2/htdocs

RUN apk add --no-cache libzip-dev \
    && docker-php-ext-configure zip --with-libzip=/usr/include \
    && docker-php-ext-install zip

RUN apk add --no-cache libxml2-dev && \
    docker-php-ext-install soap

RUN printf "\n%s\n%s" "@edge http://dl-cdn.alpinelinux.org/alpine/edge/main" "@testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
  && apk --update upgrade \
  && apk add autoconf automake make gcc g++ libtool pkgconfig libmcrypt-dev re2c libressl@edge libressl-dev@edge git zlib-dev xdg-utils libpng-dev freetype-dev libjpeg-turbo-dev openssh-client libxslt-dev ca-certificates gmp-dev \
  && update-ca-certificates

RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql
RUN apk add freetype libpng  libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev
RUN docker-php-ext-install -j$(nproc) iconv 
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ 
RUN docker-php-ext-install -j$(nproc) gd
COPY php.ini /usr/local/etc/php/php.ini

#Composer
WORKDIR /usr/local/bin/composer2
RUN curl -sS https://getcomposer.org/installer | php \
  && ln -s /usr/local/bin/composer2/composer.phar /usr/local/bin/composer \
  && composer self-update --2 \
  && composer global require laravel/installer 

ENV PATH=~/.composer/vendor/bin:$PATH

WORKDIR /usr/local/apache2/htdocs

