FROM php:8.1-fpm
MAINTAINER vladvolvakov

ARG ENV_TYPE
ARG DOCKER_USER_ID
ARG DOCKER_GROUP_ID
ARG SERVER_IP

RUN mv "$PHP_INI_DIR/php.ini-$ENV_TYPE" "$PHP_INI_DIR/php.ini"
COPY php-fpm/php.$ENV_TYPE.conf /usr/local/etc/php-fpm.d/zz-php.conf
COPY php-fpm/php.$ENV_TYPE.ini /usr/local/etc/php/conf.d/php-addition.ini

RUN apt-get update && apt-get upgrade -y &&  apt-get install libz-dev libzip-dev libpq-dev -y

RUN docker-php-ext-install -j$(nproc) zip \
   && docker-php-ext-install -j$(nproc) opcache \
   && docker-php-ext-install -j$(nproc) pdo_mysql \
   && docker-php-ext-install -j$(nproc) pgsql \
   && docker-php-ext-install -j$(nproc) pdo_pgsql

RUN pecl install redis-5.3.7 && docker-php-ext-enable redis

RUN if [ "$ENV_TYPE" = "development" ]; then pecl install xdebug-3.1.4 && docker-php-ext-enable xdebug; fi

WORKDIR /var/www/html

RUN userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi &&\
    groupadd -g $DOCKER_USER_ID www-data &&\
    useradd -l -u $DOCKER_GROUP_ID -g www-data www-data &&\
    install -d -m 0755 -o www-data -g www-data /var/www/html


WORKDIR /home
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; php composer-setup.php; php -r "unlink('composer-setup.php');"

WORKDIR /var/www/html

USER $DOCKER_USER_ID:$DOCKER_GROUP_ID

CMD /home/composer.phar install; php-fpm
