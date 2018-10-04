FROM alpine:3.8
MAINTAINER Simon Ball <simon.ball@aencoin.com>

RUN apk --update add nginx

COPY server/app /var/www/html
COPY nginx/overrides.conf /etc/nginx/modules/overrides.conf
COPY nginx/site.conf /etc/nginx/conf.d/default.conf

CMD ["nginx", "-g", "daemon off;"]
