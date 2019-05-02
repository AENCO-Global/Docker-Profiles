FROM aenco/catapult:toolchain-v0.1 as builder

COPY . /var/app/src

WORKDIR /var/app/src/_build

# ENV BOOST_ROOT /usr/local/include/boost
ENV GTEST_ROOT=/usr/local/include/gtest
ENV cppzmq_DIR=/usr/local/share/cmake/cppzmq
ENV ROCKSDB_ROOT_DIR=/usr/local/include/rocksdb
ENV BSONCXX_ROOT=/usr/local/lib
ENV MONGOCXX_ROOT=/usr/local/lib

RUN cmake \
	-DCMAKE_BUILD_TYPE=RelWithDebugInfo \
	-DCMAKE_CXX_FLAGS="-pthread" \
	-DBOOST_ROOT=/usr/lib/x86_64-linux-gnu \
	# -DBSONCXX_LIB=/usr/local/lib/libbsoncxx.so \
	# -DMONGOCXX_LIB=/usr/local/lib/libmongocxx.so \
	-DPYTHON_EXECUTABLE=/usr/bin/python3 \
	.. && \
	make publish && make -j4

# Prepare a folder of libraries for pickup
RUN mkdir -p /tmp/deps && mkdir -p /tmp/localdep \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_atomic* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_chrono* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_date_time* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_filesystem* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_log* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_program_options* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_regex* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_system* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_thread* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libboost_timer* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libgflags* /tmp/deps \
	&& cp /usr/local/lib/librocksdb* /tmp/deps \
	&& cp /usr/lib/x86_64-linux-gnu/libsnappy* /tmp/deps \
	&& cp /usr/local/lib/libzmq* /tmp/deps \
	# Dependencies which catapult will only launch if they seem to be in the same
	# place as they are during compilation
	&& cp /usr/local/lib/libmongo* /tmp/localdep \
	&& cp /usr/local/lib/libbson* /tmp/localdep

FROM ubuntu:bionic

RUN mkdir -p /root/.aen

COPY --from=builder /var/app/src/_build/bin/ /root/.aen/chain
COPY --from=builder /tmp/deps/ /root/.aen/chain
COPY --from=builder /tmp/localdep/ /usr/local/lib

# pull in extra dependency from the package maintainer
RUN apt-get update && apt-get upgrade -y && apt-get install -y libicu60

WORKDIR /root/.aen/chain
CMD ["/root/.aen/chain/catapult.server", .."]
