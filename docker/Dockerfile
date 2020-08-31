FROM php:7.3-apache

MAINTAINER Angela e Rafael

RUN apt-get update && apt-get install vim libxml2-dev libonig-dev libpng-dev libzip-dev zip libcurl3-dev libfreetype6 libfreetype6-dev libxslt1-dev unzip git -y

RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

RUN apt-get install -y libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev \
    libfreetype6-dev
RUN docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
    --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir
RUN docker-php-ext-install gd

RUN pecl install xdebug
RUN echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20180731/xdebug.so"' >> /usr/local/etc/php/php.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >>  /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=docker" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_log=/var/log/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.default_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN apt-get install -y zlib1g-dev libicu-dev g++ \
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl

RUN docker-php-ext-install bcmath

RUN docker-php-ext-install mbstring
RUN docker-php-ext-install xsl
RUN docker-php-ext-install soap

RUN apt-get install telnet -y

RUN docker-php-ext-install pdo_mysql zip

RUN apt-get install libmcrypt-dev -y
RUN pecl install mcrypt-1.0.3
RUN docker-php-ext-enable mcrypt

RUN docker-php-ext-install sockets

RUN apt-get install -y \
        librabbitmq-dev \
        libssh-dev \
    && pecl install amqp \
    && docker-php-ext-enable amqp

RUN docker-php-ext-install ctype
RUN docker-php-ext-install dom
RUN docker-php-ext-install hash
RUN docker-php-ext-install iconv
RUN docker-php-ext-install simplexml

RUN a2enmod rewrite

RUN apt-get install -y \
    libpq-dev \
    libmemcached-dev

RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y libmemcached-dev \
    && rm -rf /var/lib/apt/lists/* \
    && MEMCACHED="`mktemp -d`" \
    && curl -skL https://github.com/php-memcached-dev/php-memcached/archive/master.tar.gz | tar zxf - --strip-components 1 -C $MEMCACHED \
    && docker-php-ext-configure $MEMCACHED \
    && docker-php-ext-install $MEMCACHED \
    && rm -rf $MEMCACHED
