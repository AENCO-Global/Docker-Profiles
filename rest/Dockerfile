FROM node:6-alpine
MAINTAINER Simon Ball<simon.ball@aencoin.com>
EXPOSE 3000

COPY server/rest /var/www/html

WORKDIR /var/www/html
RUN npm install

CMD ["/usr/local/bin/npm", "start"]
