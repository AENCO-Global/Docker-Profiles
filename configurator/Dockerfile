FROM ubuntu:bionic

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  ca-certificates \
  composer \
  curl \
  php7.2-cli \
  php7.2-gd \
  php7.2-gmp \
  php7.2-mysql \
  php7.2-fpm \
  php7.2-bcmath \
  php7.2-intl \
  php7.2-mbstring \
  && mkdir -p /etc/php/7.2/fpm/pool.d

COPY fpm.conf /etc/php/7.2/fpm/php-fpm.conf
COPY php.ini /etc/php/7.2/fpm/php.ini
COPY pool.conf /etc/php/7.2/fpm/pool.d/www.conf

EXPOSE 9000
CMD ["php-fpm7.2"]
