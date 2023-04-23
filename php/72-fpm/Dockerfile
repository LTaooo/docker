FROM php:7.2.34-fpm-alpine3.12

ENV TZ=Asia/Shanghai \
    COMPOSER_VERSION=${COMPOSER_VERSION:-"2.3.10"}

# 配置阿里云镜像源，加快构建速度
RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories

# PHPIZE_DEPS 包含 gcc g++ 等编译辅助类库，完成编译后删除，PHPIZE_DEPS是php官方镜像中提供的变量
RUN apk update \
    && apk add --no-cache $PHPIZE_DEPS \
    && apk add --no-cache libstdc++ libzip-dev \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del $PHPIZE_DEPS
# 这里的扩展可以直接使用docker-php-ext-install安装
RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) mysqli \
    && wget -nv -O /usr/local/bin/composer https://github.com/composer/composer/releases/download/${COMPOSER_VERSION}/composer.phar \
    && chmod u+x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && apk del --purge *-dev \
    && rm -rf /var/cache/apk/*

# 设置时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 设置fpm配置, 当存在多个fpm配置时，www.conf是最后一个被加载的配置文件
copy ./fpm.conf /usr/local/etc/php-fpm.d/www.conf

# 默认值：/usr/local/etc/php
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# 工作目录
WORKDIR /var/www/html

# 暴露9000端口
EXPOSE 9000

# 启动php-fpm
CMD ["php-fpm"]

