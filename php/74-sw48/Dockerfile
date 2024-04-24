FROM phpswoole/swoole:4.8-php7.4-alpine

RUN \
    apk update && \
    apk add --no-cache libstdc++ tzdata $PHPIZE_DEPS && \
    curl -sfL https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
    chmod +x /usr/bin/composer                                                                     && \
    composer self-update --clean-backups --2                                    && \
    cd /usr/local/bin  && \
    pecl channel-update pecl.php.net && \
    pecl install redis && \
    docker-php-ext-enable redis && \
    docker-php-ext-install pdo_mysql && \
    docker-php-ext-install mysqli && \
    apk del $PHPIZE_DEPS && \
    rm -rf /var/cache/apk/*

WORKDIR "/www"