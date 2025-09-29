FROM ghcr.io/ksdpmx/basebuilder:5ae0986 AS builder

SHELL ["/bin/bash", "-c"]
WORKDIR /root
RUN apt update && \
    apt install -y --no-install-recommends \
      arping \
      dh-python \
      pkg-kde-tools \
      ethtool \
      inetutils-ping \
      iperf \
      libbpf-dev \
      libedit-dev \
      libelf-dev \
      libfl-dev \
      libzip-dev \
      libluajit-5.1-dev \
      luajit  \
      python3-netaddr \
      python3-pyroute2 \
      zip \
      libpolly-19-dev && \
    git clone https://github.com/iovisor/bcc && \
    mkdir bcc/build; cd bcc/build && \
    cmake .. && \
    make -j$(nproc) && \
    cd src/python/bcc-python3 && \
    pip3 wheel . -w /wheels
