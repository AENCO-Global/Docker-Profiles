FROM ubuntu:bionic
MAINTAINER Simon Ball <simon.ball@aencoin.com>

ARG CMAKE_VERSION=3.11.1
ARG ROCKSB_VERSION=5.14.fb
ARG MONGO_C_VERSION=1.13.0
ARG MONGO_CXX_VERSION=3.4.0
ARG LIBZMQ_VERSION=4.2.3
ARG LIBZMQ_CPP_VERSION=4.2.3
ARG GOOGLE_BENCHMARK_VERSION=1.4.1
ARG GOOGLE_TEST_VERSION=1.8.0
# Make sure these two values are aligned
ARG LIBBOOST_VERSION=1.69.0
ARG LIBBOOST_REFERENCE=1_69_0
ARG LIBGFLAGS_VERSION=2.2.1
ARG LIBSNAPPY_VERSION=1.1.7
# Possible options for below include "RelWithDebugInfo" "Release"
ARG BUILD_TYPE=Release
ARG WORK_PATH=/tmp
WORKDIR /tmp

RUN mkdir -p $WORK_PATH \
  && apt-get update -y && apt-get upgrade -y && apt-get install -y --no-install-recommends \
  autoconf \
  automake \
  build-essential \
  ca-certificates \
	cmake \
  curl \
	g++ \
  git \
  libatomic-ops-dev \
  libgflags-dev \
  libunwind-dev \
  libsnappy-dev \
  nano \
  ninja-build \
  python3 \
  python3-ply \
  xz-utils \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# BOOST
RUN curl -o boost_${LIBBOOST_REFERENCE}.tar.gz -SL \
    https://dl.bintray.com/boostorg/release/${LIBBOOST_VERSION}/source/boost_${LIBBOOST_REFERENCE}.tar.gz \
    && tar -xzf boost_${LIBBOOST_REFERENCE}.tar.gz \
    && cd boost_${LIBBOOST_REFERENCE} && ls -la && pwd \
    && ./bootstrap.sh --prefix=/usr/local/boost \
    && ./b2 --prefix=/usr/local/boost -j 4 stage release \
    && ./b2 install --prefix=/usr/local/boost

# GOOGLE TEST
RUN cd ${WORK_PATH} \
  && git clone --single-branch --branch release-${GOOGLE_TEST_VERSION} https://github.com/google/googletest.git gtest \
  && cd gtest && mkdir _build && cd _build \\
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_BUILD_TYPE=Release -DCMAKE_POSITION_INDEPENDENT_CODE=ON .. \
  && make -j4 \
  && make install

# GOOGLE BENCHMARK
RUN cd ${WORK_PATH} \
  && git clone --single-branch --branch v${GOOGLE_BENCHMARK_VERSION} https://github.com/google/benchmark.git gbenchmark \
  && cd gbenchmark && mkdir _build && cd _build \
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_BUILD_TYPE=Release -DBENCHMARK_ENABLE_GTEST_TESTS=OFF .. \
  && make -j4 \
  && make install

# MONGO
RUN cd ${WORK_PATH} \
  && git clone --single-branch --branch ${MONGO_C_VERSION} https://github.com/mongodb/mongo-c-driver.git mongo-c \
  && cd mongo-c && mkdir _build && cd _build \
  && cmake -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install

# MONGOCXX
RUN cd ${WORK_PATH} \
  && git clone --single-branch --branch r${MONGO_CXX_VERSION} \
    https://github.com/mongodb/mongo-cxx-driver.git mongo-cxx \
  && cd mongo-cxx \
  && sed -i 's/kvp("maxAwaitTimeMS", count)/kvp("maxAwaitTimeMS", static_cast<int64_t>(count))/' \
    src/mongocxx/options/change_stream.cpp \
  && mkdir _build && cd _build \
  && cmake -DBOOST_ROOT=/usr/local/boost \
    -DBSONCXX_POLY_USE_BOOST=1 \
    -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install
# -DLIBBSON_DIR=/usr/local
# -DLIBMONGOC_DIR=/usr/local

# ZMQ
RUN cd ${WORK_PATH} \
  && git clone git://github.com/zeromq/libzmq.git libzmq \
  && cd libzmq && mkdir _build && cd _build \
  && cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install

# CPPZMQ
RUN cd ${WORK_PATH} \
  && git clone https://github.com/zeromq/cppzmq.git cppzmq \
  && cd cppzmq && mkdir _build && cd _build \
  && cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install

# ROCKS
RUN cd ${WORK_PATH} \
  && git clone --single-branch --branch ${ROCKSB_VERSION} https://github.com/facebook/rocksdb.git rocksdb \
  && cd rocksdb && mkdir _build && cd _build \
  && cmake -DCMAKE_BUILD_TYPE=Release -DWITH_TESTS=OFF -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install
CMD ["/usr/bin/bash"]
