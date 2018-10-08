FROM ubuntu:bionic

RUN apt-get update && apt-get install -y \
  nodejs \
  npm \
  supervisor \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# AEN Server preparation
COPY --from=aenco/master-node:core /usr/local/bin /usr/local/bin

# MongoDB preparation
RUN mkdir -p /data/db

# Monit preparation
COPY bin/ /etc/init.d
COPY bin/ /usr/local/bin
COPY supervisor /etc/supervisor/conf.d/
COPY supervisord.conf /etc/supervisor/supervisord.conf

# REST Gateway preparation
COPY --from=aenco/rest-gateway:latest /var/www/html /var/aen/rest-gateway

# REST Gateway
EXPOSE 3000
# AEN Server
EXPOSE 7900
EXPOSE 7901
EXPOSE 7902

WORKDIR /usr/bin
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
