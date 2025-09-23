FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

SHELL ["/bin/bash", "-c"]
WORKDIR /root
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      ca-certificates \
      curl \
      git \
      gnupg \
      build-essential \
      autoconf \
      automake \
      libtool \
      binutils \
      pkg-config \
      libssl-dev \
      libzstd-dev \
      zlib1g-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
