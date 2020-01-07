FROM drupal:8.7.10-fpm-alpine

RUN set -ex \
  && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libmemcached-dev zlib-dev libevent-dev \
  && docker-php-ext-install -j $(nproc) opcache \
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
  && apk del .build-deps