
FROM hyperf/hyperf:8.1-alpine-v3.16-base


ARG SW_VERSION
ARG COMPOSER_VERSION
ARG PHP_BUILD_VERSION=81

##
# ---------- env settings ----------
##
ENV SW_VERSION=${SW_VERSION:-"v5.0.3"} \
    COMPOSER_VERSION=${COMPOSER_VERSION:-"2.6.6"} \
    XLSWRITER_VERSION=${XLSWRITER_VERSION:-"1.5.8"} \
    COMPOSER_ALLOW_SUPERUSER=1 \
    #  install and remove building packages
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make php${PHP_BUILD_VERSION}-dev php${PHP_BUILD_VERSION}-pear pkgconf re2c pcre-dev pcre2-dev zlib-dev libtool automake libaio-dev openssl-dev curl-dev"

# update
RUN set -ex \
    && apk update \
    # for swoole extension libaio linux-headers
    && apk add --no-cache libstdc++ openssl git bash c-ares-dev libpq-dev php81-pdo_sqlite php81-pdo_odbc php81-mongodb php81-imap \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS unixodbc-dev sqlite-dev \
    && ln -s /usr/bin/phpize${PHP_BUILD_VERSION} /usr/local/bin/phpize \
    && ln -s /usr/bin/php-config${PHP_BUILD_VERSION} /usr/local/bin/php-config \
    # download
    && cd /tmp \
    && curl -SL "https://github.com/swoole/swoole-src/archive/${SW_VERSION}.tar.gz" -o swoole.tar.gz \
    && ls -alh \
    # php extension:swoole
    && cd /tmp \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-openssl --enable-swoole-curl --enable-cares --enable-swoole-pgsql --enable-swoole-sqlite --with-swoole-odbc=unixodbc,/usr \
        && make -s -j$(nproc) && make install \
    ) \
    && echo "memory_limit=1G" > /etc/php${PHP_BUILD_VERSION}/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /etc/php${PHP_BUILD_VERSION}/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" > /etc/php${PHP_BUILD_VERSION}/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php${PHP_BUILD_VERSION}/conf.d/50_swoole.ini \
    # php extension:xls
    && curl -fsSL "https://pecl.php.net/get/xlswriter-${XLSWRITER_VERSION}.tgz" -o xlswriter.tgz \
    && mkdir -p /tmp/xlswriter \
    && tar -xf xlswriter.tgz -C /tmp/xlswriter --strip-components=1 \
    && ( \
        cd /tmp/xlswriter \
        && phpize \
        && ./configure --enable-reader \
        && make -s -j$(nproc) && make install \
    ) \
    && echo "extension=xlswriter.so" > /etc/php${PHP_BUILD_VERSION}/conf.d/xlswriter.ini \
    # install composer
    && wget -nv -O /usr/local/bin/composer https://github.com/composer/composer/releases/download/${COMPOSER_VERSION}/composer.phar \
    && chmod u+x /usr/local/bin/composer \
    # ---------- clear works ----------
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/local/bin/php* \
    # php info
    && php -v \
    && php -m \
    && php --ri swoole \
    && php --ri Zend\ OPcache \
    && composer \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"