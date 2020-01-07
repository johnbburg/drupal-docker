FROM drush/drush:8

RUN set -ex \
  && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS coreutils freetype-dev libjpeg-turbo-dev postgresql-dev libzip-dev libmemcached-dev zlib-dev libevent-dev \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
  && docker-php-ext-install -j $(nproc) gd pdo_mysql pdo_pgsql zip opcache \
  && pecl install xdebug \
  && pecl install memcached \
  && docker-php-ext-enable memcached \
  && runDeps="$(\
  scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
    | tr ',' '\n' \
    | sort -u \
    | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
  )" \
  && apk add --virtual .docker-phpexts-rundeps $runDeps \
  && apk del .build-deps \
  && apk add --no-cache mysql-client openssh rsync \
  && echo 'memory_limit = -1' >> /usr/local/etc/php/php-cli.ini