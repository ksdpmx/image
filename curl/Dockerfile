FROM ghcr.io/ksdpmx/basebuilder:bc6922f AS builder

ARG VERSION="8.16.0"

SHELL ["/bin/bash", "-c"]
WORKDIR /root
RUN curl -LO https://github.com/curl/curl/releases/download/curl-$(echo $VERSION | sed 's/\./_/g')/curl-${VERSION}.tar.xz && \
    curl -LO https://github.com/curl/curl/releases/download/curl-$(echo $VERSION | sed 's/\./_/g')/curl-${VERSION}.tar.xz.asc && \
    curl -LO https://daniel.haxx.se/mykey.asc && \
    gpg --import mykey.asc && \
    gpg --verify curl-${VERSION}.tar.xz.asc curl-${VERSION}.tar.xz && \
    tar -xf curl-${VERSION}.tar.xz && \
    cd /root/curl-${VERSION} && \
    autoreconf -fi && \
    ./configure --disable-shared --enable-static \
      --disable-docs --disable-manual \
      --disable-verbose \
      --disable-unity \
      --with-openssl \
      --enable-unix-sockets \
      --disable-ipv6 \
      --without-libpsl \
      --without-libssh2 \
      --disable-ldap --disable-ldaps \
      --disable-ipfs \
      --disable-kerberos-auth && \
    make -j$(nproc) LDFLAGS="-static -all-static"

FROM gcr.io/distroless/static-debian12:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /root/curl-${VERSION}/src/curl /usr/local/bin/curl
