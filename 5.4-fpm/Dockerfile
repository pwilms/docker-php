FROM php:5.4-fpm
MAINTAINER Alterway <iac@alterway.fr>

RUN apt-get update && \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libgmp-dev \
        libxml2-dev \
        zlib1g-dev \
        libncurses5-dev \
        libldap2-dev \
        libicu-dev \
        libmemcached-dev \
        libcurl4-openssl-dev \
        libssl-dev \
        php-pear \
        curl \
        ssmtp && \
    rm -rf /var/lib/apt/lists/* && \
    ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mysql && \
    docker-php-ext-install ldap && \
    docker-php-ext-install soap && \
    docker-php-ext-install intl && \
    docker-php-ext-install mcrypt && \
    docker-php-ext-install gd && \
    docker-php-ext-install gmp && \
    docker-php-ext-install bcmath && \
    docker-php-ext-install mbstring && \
    docker-php-ext-install zip && \
    docker-php-ext-install pcntl && \
    pecl install mongo && \
    pecl install memcached && \
    pecl install redis && \
    pecl install xdebug

ENV LOCALTIME Europe/Paris

RUN rm $PHP_INI_DIR/conf.d/docker-php-ext* && \
    echo 'sendmail_path = /usr/sbin/ssmtp -t' >> $PHP_INI_DIR/conf.d/00-default.ini && \
    echo "\ninclude=/usr/local/etc/php-fpm.d/*.conf" >> /usr/local/etc/php-fpm.conf && \
    mkdir -p /usr/local/etc/php-fpm.d

ADD https://blackfire.io/api/v1/releases/probe/php/linux/amd64/54 /tmp/blackfire-probe.tar.gz
RUN tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp && \
    mv /tmp/blackfire-*.so `php -r "echo ini_get('extension_dir');"`/blackfire.so && \
    rm /tmp/blackfire-probe.tar.gz

COPY docker-entrypoint.sh /entrypoint.sh

WORKDIR /var/www

ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]