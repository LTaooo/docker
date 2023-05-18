
FROM ltaooo/php:80-fpm

ARG timezone
ENV TIMEZONE=${timezone:-"Asia/Shanghai"}

RUN set -ex \
    && apk update \
    && apk add --no-cache openssh-client git \
    && cd /var/www/html \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer create-project playbloom/satisfy:3.5.1 \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    # ---------- clear works ----------
    && composer clearcache \
    && rm -rf /var/cache/apk/* /tmp/*  \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

COPY ./satis.json /var/www/html/satisfy/satis.json

WORKDIR /var/www/html/satisfy

# 暴露9000端口
EXPOSE 9000

# 启动
ENTRYPOINT ["php", "-S", "0.0.0.0:9000", "-t", "./public/"]