FROM ubuntu:bionic

RUN apt-get update && apt-get install -y \
  mongodb \
  monit \
  nginx \
  nodejs \
  npm \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

# AEN Server preparation
COPY --from=aenco/master-node /usr/local/bin /usr/local/bin

# Monit preparation
COPY bin/ /etc/init.d
COPY monit/monitrc /etc/monit/monitrc
RUN chmod +x /etc/init.d/* \
  && chown root:root /etc/monit/monitrc \
  && chmod 700 /etc/monit/monitrc \
  && mkdir -p /var/run/monit

# REST Gateway preparation
COPY --from=aenco/rest-gateway:latest /var/www/html /var/aen/rest-gateway

# Block Explorer preparation
COPY --from=aenco/block-explorer:latest /var/www/html /var/aen/block-explorer
COPY nginx/overrides.conf /etc/nginx/modules-enabled/overrides.conf
COPY nginx/block-explorer.conf /etc/nginx/sites-available/default

# REST Gateway
EXPOSE 3000
# Block Explorer
EXPOSE 80
# AEN Server
EXPOSE 7900
EXPOSE 7901
EXPOSE 7902

WORKDIR /usr/bin
CMD ["/usr/bin/monit", "-I"]
