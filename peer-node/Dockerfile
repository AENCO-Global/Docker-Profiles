FROM ubuntu:bionic

RUN apt-get update && apt-get install -y \
  libicu60 \
  mongodb \
  net-tools \
  nodejs \
  npm \
  supervisor \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && npm i -g yarn

# AEN Server
COPY --from=aenco/master-node:core /root/.aen/chain/ /root/.aen/chain
COPY --from=aenco/master-node:core /usr/local/lib/ /usr/local/lib

# Chain agent
COPY --from=aenco/chain-agent:latest /go/src/app/aen.agent /root/.aen/bin/aen.agent

# REST Gateway
COPY --from=aenco/rest-gateway:latest /var/www/html /root/.aen/rest-gateway

# Path preparation
RUN mkdir -p /data/db \
  && mkdir -p /root/.aen/bin \
  && mkdir -p /root/.aen/supervisor \
  && mkdir -p /root/.aen/var

# Supervisor configuration
COPY bin/ /root/.aen/bin
COPY supervisor/ /root/.aen/supervisor

# REST Gateway
EXPOSE 3000
# AEN Server
EXPOSE 7900
EXPOSE 7901
EXPOSE 7902

WORKDIR /usr/bin
VOLUME ["/data/db", "/root/.aen/data"]
CMD ["/usr/bin/supervisord", "-c", "/root/.aen/supervisor/supervisord.conf"]
