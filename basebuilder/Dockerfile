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
      clang clang-format libclang-dev libclang-cpp-dev \
      cmake \
      autoconf automake \
      llvm llvm-dev \
      libtool \
      binutils \
      pkg-config \
      linux-libc-dev \
      libssl-dev \
      libzstd-dev \
      zip libzip-dev \
      zlib1g-dev \
      flex bison \
      dpkg-dev \
      python3-pip \
      python3-setuptools && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
