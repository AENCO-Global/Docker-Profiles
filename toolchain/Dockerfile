FROM ubuntu:bionic
MAINTAINER Simon Ball <simon.ball@aencoin.com>

ARG CMAKE_VERSION=3.11.1
ARG ROCKSB_VERSION=5.14.fb
ARG MONGO_C_VERSION=1.11.0
ARG MONGO_CXX_VERSION=3.3
ARG LIBZMQ_VERSION=4.2.3
ARG LIBZMQ_CPP_VERSION=4.2.3
# Make sure these two values are aligned
ARG LIBBOOST_VERSION=1.64.0
ARG LIBBOOST_REFERENCE=1_64_0
ARG LIBGFLAGS_VERSION=2.2.1
ARG LIBSNAPPY_VERSION=1.1.7
# Possible options for below include "RelWithDebugInfo" "Release"
ARG BUILD_TYPE=Release
ARG WORK_PATH=/tmp

RUN mkdir -p $WORK_PATH \
  && apt-get update -y && apt-get upgrade -y && apt-get install -y --no-install-recommends \
  autoconf \
  automake \
  apt-file \
  build-essential \
  ca-certificates \
	cmake \
  curl \
	g++ \
  git \
  googletest \
	libboost1.65-all-dev \
  libbz2-dev \
  libc6-dev \
  libgflags-dev \
  liblz4-dev \
  libsasl2-dev \
  libsnappy-dev \
  libssl-dev \
  libtool \
  libzstd-dev \
  make \
  nano \
  pkg-config \
  python-dev \
  python3 \
  software-properties-common \
  wget \
  zlib1g-dev \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# Compile Google Test
RUN cd /usr/src/googletest \
  && mkdir -p _build && cd _build \
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 \
  && make install

# rocksdb
RUN cd ${WORK_PATH} && echo Compiling RocksDB ${ROCKSB_VERSION} \
  && git clone https://github.com/facebook/rocksdb.git --branch ${ROCKSB_VERSION} --depth 1 rocksdb \
  && cd rocksdb \
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local -LDFLAGS=-lpthread . \
  && make -j4 all && make install \
  && rm -R ${WORK_PATH}/rocksdb

# mongo-c
RUN cd ${WORK_PATH} && echo Installing mongo C driver ${MONGO_C_VERSION} \
  && git clone https://github.com/mongodb/mongo-c-driver.git --branch ${MONGO_C_VERSION} --depth 1 mongo-c-driver \
  && mkdir -p mongo-c-driver/_build && cd mongo-c-driver/_build \
  && cmake -DCMAKE_BUILD_TYPE="${BUILD_TYPE}" -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 && make install \
  && rm -R ${WORK_PATH}/mongo-c-driver

# mongo-cxx
RUN cd ${WORK_PATH} && echo Installing Mongo Cxx driver ${MONGO_CXX_VERSION} \
  && git clone https://github.com/mongodb/mongo-cxx-driver.git --branch releases/v${MONGO_CXX_VERSION} --depth 1 mongo-cxx-driver \
  && mkdir -p mongo-cxx-driver/_build && cd mongo-cxx-driver/_build \
  && cmake -DCMAKE_BUILD_TYPE="${BUILD_TYPE}" -DCMAKE_INSTALL_PREFIX=/usr/local -DBSONCXX_POLY_USE_BOOST=1 .. \
  && make -j4 && make install \
  && rm -R ${WORK_PATH}/mongo-cxx-driver

# libzmq
RUN cd ${WORK_PATH} && echo Installing ZMQ ${LIBZMQ_VERSION} \
	&& git clone https://github.com/zeromq/libzmq.git --branch v${LIBZMQ_VERSION} --depth 1 libzmq \
  && mkdir -p libzmq/_build && cd libzmq/_build \
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 && make install \
  && rm -R ${WORK_PATH}/libzmq

# cppzmq
RUN cd ${WORK_PATH} && echo Installing CPPZMQ ${LIBZMQ_CPP_VERSION} && git clone https://github.com/zeromq/cppzmq.git --branch v${LIBZMQ_CPP_VERSION} --depth 1 cppzmq \
  && rm -R cppzmq/.git \
  && mkdir -p cppzmq/_build && cd cppzmq/_build \
  && cmake -DCMAKE_INSTALL_PREFIX=/usr/local .. \
  && make -j4 && make install \
  && rm -R ${WORK_PATH}/cppzmq

CMD ["/usr/bin/bash"]
