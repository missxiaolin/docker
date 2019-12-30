FROM php:7.3.6-fpm-stretch

COPY nesFile /tmp/nesFile
RUN curl -o nginx.key "http://nginx.org/keys/nginx_signing.key" &\
    mv /tmp/nesFile/sources.list /etc/apt/ && rm -f /etc/apt/sources.list.d/* &&\
    apt-get update --fix-missing && apt install gnupg1 -y &&\
    echo 'deb http://nginx.org/packages/mainline/debian/ stretch nginx' >>/etc/apt/sources.list &&\
    echo 'deb-src http://nginx.org/packages/mainline/debian/ stretch nginx' >>/etc/apt/sources.list &&\
    apt-key add nginx.key && rm -f nginx.key && apt-get update &&\
    apt-get -y --no-install-recommends install libldap-dev libpng-dev libbz2-dev libxml2-dev libxslt-dev \
        libzip-dev libssl-dev libcurl4-openssl-dev nginx nscd&&\
    mv /tmp/nesFile/nginx.conf /etc/nginx/ &&\
    mv /tmp/nesFile/nscd.conf /etc/ &&\
    mv /tmp/nesFile/default.conf /etc/nginx/conf.d/ &&\
    mv /tmp/nesFile/php-fpm-status.conf /etc/nginx/conf.d/ &&\
    mv /tmp/nesFile/composer /usr/local/bin/composer &&\
#    mv /tmp/nesFile/sky-php-agent-linux-x64 /usr/local/bin/apm_agent &&\
    mv /tmp/nesFile/php-fpm-exporter.linux.amd64 /usr/local/bin/php-fpm-exporter &&\
    mkdir -p /etc/php-fpm && mv /tmp/nesFile/php-fpm.conf /etc/php-fpm/ &&\
    echo root:phproot.123 | chpasswd &&\
    useradd  gs -m -s /bin/bash && mkdir -p /app/logs && chown -R gs.gs /app &&\
    chmod u+s /usr/sbin/nginx /etc/init.d/nscd /usr/sbin/nscd &&\
    echo "export PATH=$PATH:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin" >> /home/gs/.bashrc &&\
    su -l gs -c "composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/" &&\
    cd /tmp/nesFile/php-psr-0.6.1 &&\
    /usr/local/bin/phpize && ./configure --with-php-config=/usr/local/bin/php-config &&\
    make && make test && make install && docker-php-ext-enable psr &&\
    cd /tmp/nesFile/cphalcon-3.4.4/build &&\
    ./install --phpize /usr/local/bin/phpize --php-config /usr/local/bin/php-config &&\
    cd /tmp/nesFile/SkyAPM-php-sdk-3.2.0 && phpize && ./configure && make && make install &&\
    docker-php-ext-install calendar exif gd gettext ldap mysqli pcntl pdo_mysql sockets wddx xsl zip opcache &&\
    pecl install igbinary redis swoole &&\
    docker-php-ext-enable igbinary redis swoole phalcon &&\
## skywalking
    cd && docker-php-source delete &&\
    apt-get purge gnupg1 -y && apt-get clean &&\
    rm -rf /tmp/* /var/cache/apt/archives/*
