FROM ubuntu:xenial

ARG OPENSSL_SRC=https://www.openssl.org/source/openssl-1.1.0f.tar.gz
ARG CURL_SRC=https://github.com/curl/curl/releases/download/curl-7_59_0/curl-7.59.0.zip

RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y -f build-essential \
        software-properties-common \
        g++-multilib \
        gcc-multilib \
        libc6-dev \
        libc6-dev:i386 \
        unzip \
        zip \
        wget
WORKDIR /root/

# build openssl
RUN wget ${OPENSSL_SRC} && \
    tar xzvf openssl-1.1.0f.tar.gz && \
    cd openssl-1.1.0f && \
    setarch i386 ./config --prefix=/usr/local/ \
    -m32 \
#    no-ui \
    no-ssl3 \
    shared && \
    make && \
    make install && \
    cd ../ && \
    rm -Rf openssl-1.1.0f openssl-1.1.0f.tar.gz

# build curl
RUN wget ${CURL_SRC} && \
   unzip curl-7.59.0.zip && \
   cd curl-7.59.0 && \
   CFLAGS=-m32 ./configure --prefix=/usr/local/ \
       --without-librtmp \
		--host=i686-pc-linux-gnu \
		--disable-ftp \
		--disable-file \
		--disable-ldap \
		--disable-ldaps \
		--disable-rtsp \
		--disable-dict \
		--disable-telnet \
		--disable-tftp \
		--disable-pop3 \
		--disable-imap \
		--disable-smb \
		--disable-smtp \
		--disable-gopher \
		--disable-manual \
		--enable-ipv6 \
		--disable-pthreads \
		--enable-crypto-auth \
		--enable-cookies \
		--without-zlib \
		--disable-threaded-resolver \
		--without-brotli \
		--with-ssl=/usr/local/lib && \
		make && \
		make install && \
        cd ../ && \
        rm -Rf curl-7.59.0 curl-7.59.0.zip
