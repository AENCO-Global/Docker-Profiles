FROM aenco/master-node:toolchain as builder

COPY server /var/app/src

WORKDIR /var/app/src/_build

ENV BOOST_ROOT /usr/local/include/boost
ENV GTEST_ROOT=/usr/local/include/gtest
ENV cppzmq_DIR=/usr/local/share/cmake/cppzmq
ENV ROCKSDB_ROOT_DIR=/usr/local/include/rocksdb

RUN cmake -DCMAKE_BUILD_TYPE=RelWithDebugInfo \
-DBSONCXX_LIB:FILEPATH=/usr/local/lib/libbsoncxx.so \
-DMONGOCXX_LIB:FILEPATH=/usr/local/lib/libmongocxx.so \
-DPYTHON_EXECUTABLE=/usr/bin/python3 \
.. && \
make publish && make -j4

FROM ubuntu:bionic

COPY --from=builder /var/app/src/_build/bin/ /usr/local/bin
COPY --from=builder /usr/local/lib /usr/local/bin

WORKDIR /usr/local/bin
CMD ["/usr/local/bin/aen.server", "/var/aen"]

RUN apt-get update && apt-get install \
  mongodb \
  monit \
  nginx \
  nodejs

COPY bin/ /usr/local/bin
COPY monit/monitrc /etc/monit/monitrc

# Block Explorer specific
COPY --from=aenco/node-explorer:latest /var/www/html /var/aen/block-explorer
COPY docker_support/nginx-block-explorer.conf /etc/nginx/sites-available/default

# Modify Nginx tp forward web requests to the node explorer

monit logs - /var/log/monit
catapult logs - /usr/loca/bin/logs

monit -I
-I Run in foreground
