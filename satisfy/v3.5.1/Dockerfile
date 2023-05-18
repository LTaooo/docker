
FROM hyperf/hyperf:8.0-alpine-v3.14-dev

ARG timezone
ENV TIMEZONE=${timezone:-"Asia/Shanghai"}

RUN set -ex \
    && apk update \
    && apk add --no-cache openssh-client git tzdata \
    && mkdir /www && cd /www \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer create-project playbloom/satisfy:3.5.1 \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    # ---------- clear works ----------
    && composer clearcache \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/local/bin/php* \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

WORKDIR /www/satisfy

ENTRYPOINT ["php", "-S", "0.0.0.0:9511", "-t", "public"]