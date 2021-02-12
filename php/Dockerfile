FROM php:7.4-fpm

ARG USERNAME=giovanny
#"<WATEVER USER NAME YOU WANT>"

WORKDIR /usr/local/apache2/htdocs

RUN apt-get update && apt-get install -y \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libpng-dev \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd

RUN apt-get update && apt-get install -y \
  libzip-dev \
  zip \
  libxml2-dev \
  && docker-php-ext-install zip soap mysqli pdo_mysql

#GD
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j "$(nproc)" gd

COPY php.ini /usr/local/etc/php/php.ini

#Composer
RUN mkdir /home/${USERNAME}
WORKDIR /usr/local/bin/composer2
RUN curl -sS https://getcomposer.org/installer | php \
  && ln -s /usr/local/bin/composer2/composer.phar /usr/local/bin/composer \
  && composer self-update --2 \
  && composer global require laravel/installer \
  && mv /root/.composer/ /home/${USERNAME}

ENV PATH=/home/${USERNAME}/.composer/vendor/bin:$PATH

RUN groupadd -g 1000 ${USERNAME}
RUN useradd -d /home/${USERNAME} -s /bin/bash -u 1000 -g 1000 ${USERNAME}
RUN chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}
RUN chown -R ${USERNAME}:${USERNAME} /usr/local/apache2/htdocs
USER ${USERNAME}

WORKDIR /usr/local/apache2/htdocs

## alias
RUN echo 'alias l="ls -la"' >> ~/.bashrc