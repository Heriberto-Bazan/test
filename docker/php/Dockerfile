FROM php:7.4.6-fpm

ARG UID
EXPOSE $UID

RUN adduser -u ${UID} --disabled-password --gecos "" userapp
RUN mkdir /home/userapp/.ssh
RUN chown -R userapp:userapp /home/userapp/
RUN echo "StrictHostKeyChecking no" >> /home/userapp/.ssh/config
RUN echo "export COLUMNS=300" >> /home/userapp/.bashrc
RUN echo "alias sf=/appdata/www/bin/console" >> /home/userapp/.bashrc

COPY ./php.ini /usr/local/etc/php/php.ini

RUN apt-get update \
    && apt-get install -y git acl openssl openssh-client wget zip vim librabbitmq-dev libssh-dev \
    && apt-get install -y libpng-dev zlib1g-dev libzip-dev libxml2-dev libicu-dev \
    && docker-php-ext-install intl pdo pdo_mysql zip gd soap bcmath sockets \
    && pecl install xdebug-3.1.1 amqp \
    && docker-php-ext-enable --ini-name 05-opcache.ini opcache amqp xdebug

RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer
RUN composer self-update

RUN wget https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v3.2.1/php-cs-fixer.phar -O php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

RUN mkdir -p /appdata/www

WORKDIR /appdata/www