# OS Debian 10
FROM php:7.4-fpm

#PHP Modules
RUN apt-get update && apt-get install -y \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libpng-dev \
  libzip-dev \
  zip \
  libxml2-dev \
  && docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-install zip soap mysqli pdo_mysql

#Microsoft ODBC Driver & PDO_SQLSRV
RUN apt-get update && apt-get install -y gnupg2 \
  && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
  && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
  && apt-get update \
  && ACCEPT_EULA=Y apt-get install -y msodbcsql17 \
  && ACCEPT_EULA=Y apt-get install -y mssql-tools \
  && apt-get install -y unixodbc-dev \
  && apt-get install -y libgssapi-krb5-2

ENV PATH=/opt/mssql-tools/bin:$PATH

RUN apt-get update \ 
  && apt-get -y install unixodbc-dev \
  && pecl install sqlsrv pdo_sqlsrv

#ODBC Driver for Oracle & PHP_OCI8
#RUN apt-get update && apt-get -y install wget bsdtar libaio1 && \
#wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-basic-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
#wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sdk-linux.x64-12.2.0.1.0.zip | bsdtar -xvf-  -C /usr/local && \
#wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sqlplus-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
#ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
#ln -s /usr/local/instantclient/libclntsh.so.* /usr/local/instantclient/libclntsh.so && \
#ln -s /usr/local/instantclient/lib* /usr/lib && \
#ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus && \
#docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient && \
#docker-php-ext-install oci8 && \
#rm -rf /var/lib/apt/lists/* && \
#php -v
#
#RUN wget http://php.net/distributions/php-7.4.0.tar.gz && \
#mkdir php_oci && \
#mv php-7.4.0.tar.gz ./php_oci
#WORKDIR /usr/local/apache2/htdocs/php_oci
#RUN tar xfvz php-7.4.0.tar.gz
#WORKDIR /usr/local/apache2/htdocs/php_oci/php-7.4.0/ext/pdo_oci
#RUN phpize && \
#./configure --with-pdo-oci=instantclient,/usr/local/instantclient,12.1 && \
#make && \
#make install && \
#echo extension=pdo_oci.so > /usr/local/etc/php/conf.d/pdo_oci.ini && \


COPY php.ini /usr/local/etc/php/php.ini

#COMPOSER
WORKDIR /usr/local/bin/composer2
RUN curl -sS https://getcomposer.org/installer | php \
  && ln -s /usr/local/bin/composer2/composer.phar /usr/local/bin/composer \
  && composer self-update --2 \
  && composer global require laravel/installer \
  && mv /root/.composer/ /home/${USERNAME}
ENV PATH=/home/${USERNAME}/.composer/vendor/bin:$PATH

WORKDIR /usr/local/apache2/htdocs

#HOST MACHINE USER
ARG USERNAME=giovanny
RUN groupadd -g 1000 ${USERNAME}
RUN mkdir /home/${USERNAME}
RUN useradd -d /home/${USERNAME} -s /bin/bash -u 1000 -g 1000 ${USERNAME}
RUN chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}
USER ${USERNAME}

# ALIAS
RUN echo 'alias l="ls -la"' >> ~/.bashrc