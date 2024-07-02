FROM rabbitmq:3.13-management-alpine

# rabbit默认集成的插件, 容器中可以通过rabbitmq-plugins list查看支持的插件, 直接enable就可以
RUN rabbitmq-plugins enable --offline rabbitmq_management rabbitmq_prometheus

# 安装延时插件
RUN set -ex \
    && apk update \
    && apk add --no-cache wget \
    # 插件必须放到/plugins目录, 才可以安装
    && cd /plugins \
    && wget -nv -O rabbitmq_delayed_message_exchange-3.13.0.ez https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/download/v3.13.0/rabbitmq_delayed_message_exchange-3.13.0.ez \
    && rabbitmq-plugins enable rabbitmq_delayed_message_exchange \
    && apk del --purge wget \
    && rm -rf /var/cache/apk/*
