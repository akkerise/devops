FROM laradock/workspace:latest-7.3

RUN apt-get install -yqq php7.3-xml php7.3-dom php-redis

USER root

COPY ./php7.3/crontab/cron.d /etc/cron.d

RUN chmod -R 644 /etc/cron.d && crontab /etc/cron.d/crontab 

RUN ln -s /usr/bin/php /usr/local/bin/php

COPY ./php/* /etc/php/7.3/cli/conf.d/

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

WORKDIR /var/www/public_html


