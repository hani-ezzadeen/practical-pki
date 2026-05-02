FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y wget vim build-essential \
    softhsm2 opensc \
    meson pkgconf && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/openssl/openssl/releases/download/openssl-4.0.0/openssl-4.0.0.tar.gz && \
    tar -xzf openssl-4.0.0.tar.gz && \
    cd openssl-4.0.0 && \
    ./config enable-fips && \
    make && \
    make install && \
    make install_fips

RUN cp /openssl-4.0.0/libssl.so.4 /lib/x86_64-linux-gnu && cp /openssl-4.0.0/libcrypto.so.4 /lib/x86_64-linux-gnu

ENV PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig

RUN wget https://github.com/openssl-projects/pkcs11-provider/releases/download/v1.2.0/pkcs11-provider-1.2.0.tar.xz && \
    tar -xvJf pkcs11-provider-1.2.0.tar.xz && \
    cd pkcs11-provider-1.2.0 && \
    meson setup builddir \
    --prefix=/usr/local \
    --libdir=lib64 \
    -Dbuild_info="Built for OpenSSL 4.0.0" && \
    meson install -C builddir

RUN mkdir /opt/workspace
WORKDIR /opt/workspace
