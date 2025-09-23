ARG VERSION="8.16.0"

FROM ghcr.io/ksdpmx/basebuilder:bc6922f AS builder

ARG VERSION

SHELL ["/bin/bash", "-c"]
WORKDIR /root
RUN apt update && \
    apt install -y --no-install-recommends \
      libbrotli-dev \
      libnghttp2-dev \
      libngtcp2-dev \
      libngtcp2-crypto-gnutls-dev \
      libnghttp3-dev && \
    curl -LO https://github.com/curl/curl/releases/download/curl-$(echo $VERSION | sed 's/\./_/g')/curl-${VERSION}.tar.xz && \
    curl -LO https://github.com/curl/curl/releases/download/curl-$(echo $VERSION | sed 's/\./_/g')/curl-${VERSION}.tar.xz.asc && \
    curl -LO https://daniel.haxx.se/mykey.asc && \
    gpg --import mykey.asc && \
    gpg --verify curl-${VERSION}.tar.xz.asc curl-${VERSION}.tar.xz && \
    tar -xf curl-${VERSION}.tar.xz && \
    cd /root/curl-${VERSION} && \
    autoreconf -fi && \
    ./configure --disable-shared --enable-static \
      --disable-manual --disable-docs \
      --enable-verbose \
      --disable-unity \
      --with-openssl \
      --enable-http \
      --disable-ftp \
      --disable-file \
      --disable-ipfs \
      --disable-ldap --disable-ldaps \
      --disable-rtsp \
      --enable-proxy \
      --disable-dict \
      --disable-telnet \
      --disable-tftp \
      --disable-pop3 \
      --disable-imap \
      --disable-smb \
      --disable-smtp \
      --disable-gopher \
      --disable-mqtt \
      --disable-libcurl-option \
      --with-zlib \
      --with-brotli \
      --with-zstd \
      --disable-ipv6 \
      --without-libpsl \
      --without-libgsasl \
      --without-libssh2 \
      --without-librtmp \
      --disable-versioned-symbols \
      --without-libidn2 \
      --with-nghttp2 \
      --without-libuv \
      --without-zsh-functions-dir \
      --without-fish-functions-dir \
      --enable-basic-auth \
      --enable-bearer-auth \
      --enable-digest-auth \
      --disable-kerberos-auth \
      --disable-negotiate-auth \
      --enable-aws \
      --disable-ntlm \
      --disable-tls-srp \
      --enable-unix-sockets \
      --enable-cookies  \
      --enable-socketpair \
      --enable-http-auth  \
      --enable-doh  \
      --enable-mime  \
      --enable-bindlocal  \
      --enable-form-api \
      --enable-dateparse \
      --disable-netrc  \
      --enable-progress-meter  \
      --enable-sha512-256  \
      --enable-dnsshuffle \
      --enable-get-easy-options  \
      --enable-alt-svc  \
      --enable-headers-api \
      --enable-hsts  \
      --enable-websockets && \
    make -j$(nproc) CFLAGS="-Os -s" LDFLAGS="-s -static -all-static"

FROM gcr.io/distroless/static-debian12:debug

ARG VERSION
LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /root/curl-${VERSION}/src/curl /usr/local/bin/curl

ENTRYPOINT ["/usr/local/bin/curl"]
