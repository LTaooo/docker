
FROM hyperf/hyperf:8.3-alpine-v3.19-dev

RUN set -ex  \
    # download
    && cd /tmp \
    && curl -SL "https://github.com/swoole/swoole-src/archive/v5.1.1.tar.gz" -o swoole.tar.gz \
    && ls -alh \
    # php extension:swoole
    && cd /tmp \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-openssl --enable-swoole-curl --enable-cares \
        && make -s -j$(nproc) && make install \
    ) \
    && echo "memory_limit=1G" > /etc/php83/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /etc/php83/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" > /etc/php83/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /etc/php83/conf.d/50_swoole.ini \
    && apk add --no-cache php83-intl \
    # ---------- clear works ----------
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    # php info
    && php -v \
    && php -m \
    && php --ri swoole \
    && php --ri Zend\ OPcache \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n" \
