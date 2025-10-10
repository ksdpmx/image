FROM ghcr.io/ksdpmx/basebuilder:5ae0986 AS builder

SHELL ["/bin/bash", "-c"]
WORKDIR /root
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
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

FROM docker.io/debian:sid-slim

COPY --from=builder /wheels /wheels
# links are not preserved, but it won'r break anything
COPY --from=builder /root/bcc/build/src/cc/libbcc*.a* /root/bcc/build/src/cc/libbcc*.so* /usr/local/lib/

RUN apt-get update && \
    apt-get install -y --no-install-recommends bpfcc-tools libbpfcc libbpfcc-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    cd /wheels && \
    python3 -m zipfile -e $(ls bcc-*.whl | head -n1) /wheels/bcc && \
    cp -r /wheels/bcc/* /usr/lib/python3/dist-packages/ && \
    rm -rf /wheels
