FROM ubuntu:18.04

ARG DYNOMITE_VERSION=v0.7.0

RUN apt-get update && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get install -y \
    autoconf \
    build-essential \
    dh-autoreconf \
    git \
    libssl-dev \
    libtool \
    software-properties-common \
    redis-server \
    tcl8.5 

WORKDIR /tmp
RUN git clone https://github.com/Netflix/dynomite.git
WORKDIR /tmp/dynomite
RUN git checkout tags/$DYNOMITE_VERSION

RUN autoreconf -fvi \
    && ./configure --enable-debug=log \
    && CFLAGS="-ggdb3 -O0" ./configure --enable-debug=full \
    && make \
    && make install

ADD /dynomite.yml /app/
ADD /redis.conf /app/

RUN mkdir -p /app \
    && cp src/dynomite /app \
    && rm -rf /tmp/dynomite

ADD /startup.sh /app/startup.sh

VOLUME /var/lib/redis

