

FROM hyperf/hyperf:8.0-alpine-v3.14-dev
RUN set -ex \
&& mkdir /www && cd /www \
&& composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
&& composer create-project playbloom/satisfy:3.5.1 \
# ---------- clear works ----------
&& composer clearcache \
&& rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/local/bin/php* \
&& echo -e "\033[42;37m Build Completed :).\033[0m\n"