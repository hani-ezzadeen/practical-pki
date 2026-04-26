FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y wget vim build-essential \
    softhsm2 opensc pkcs11-provider && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/openssl/openssl/releases/download/openssl-4.0.0/openssl-4.0.0.tar.gz && \
    tar -xzf openssl-4.0.0.tar.gz && \
    cd openssl-4.0.0 && \
    ./config enable-fips && \
    make && \
    make install && \
    make install_fips

RUN cp /openssl-4.0.0/libssl.so.4 /lib/x86_64-linux-gnu && cp /openssl-4.0.0/libcrypto.so.4 /lib/x86_64-linux-gnu

RUN mkdir /opt/workspace
WORKDIR /opt/workspace
