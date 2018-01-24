#Grab the latest alphine

FROM alpine

MAINTAINER JAMES COLLINS <jamescollins.jc30@gmail.com>

LABEL caddy_version="0.9.3" architecture="amd64"

ARG plugins=realip

RUN apk add --no-cache openssh git curl supervisor --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/

RUN curl  --show-error --fail --location \
      --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
      "https://caddyserver.com/download/build?os=linux&arch=amd64&features=${plugins}" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy \
 && chmod 0755 /usr/bin/caddy \
 && /usr/bin/caddy -version

# Install extensions
RUN apk add --no-cache wget ca-certificates php7-intl php7-fpm php7 php7-pdo php7-pdo_mysql php7-zip php7-gd php7-zip php7-dom php7-xml php7-xmlreader php7-phar php7-openssl php7-ctype php7-zlib php7-mbstring php7-json php7-xml php7-opcache php7-curl php7-mcrypt
RUN update-ca-certificates

COPY fpm-pool.conf /etc/php7/php-fpm.d/www-pool.conf
RUN ln -s `which php7`  /usr/bin/php
RUN ln -s `which php-fpm7`  /usr/bin/php-fpm

RUN mkdir /srv -p

WORKDIR /srv

#Grab our latest master from gitlab and install dependencies
ENV TOKEN_HEADER 'PRIVATE-TOKEN: VBmzxgkCFTEA3PTWivNo'

RUN wget  --header="$TOKEN_HEADER" "http://gitlab.com/api/v3/projects/1220927/repository/archive?sha=master"\
  -O master.tar.gz && tar -xf master.tar.gz --strip 1 -C /srv && rm master.tar.gz
RUN curl getcomposer.org/installer | php && \
    mv /srv/composer.phar /usr/bin/composer && cd /srv && \
    composer install --no-suggest  --no-dev

WORKDIR /srv/
RUN chmod 777 -R storage/ &&  chmod 777 -R bootstrap/cache/

#Install supervisord
#RUN mkdir /etc/supervisor/conf.d/   -p
COPY supervisord.conf /etc/supervisord.conf
RUN mv .env.test .env

EXPOSE 22 80

CMD [ "/usr/bin/supervisord"  , "-c" , "/etc/supervisord.conf" ]
