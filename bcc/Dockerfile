FROM ghcr.io/ksdpmx/basebuilder:48af905 AS builder

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
      luajit libluajit-5.1-dev \
      python3-netaddr \
      python3-pyroute2 \
      libpolly-19-dev \
      libelf1t64 && \
    git clone https://github.com/iovisor/bcc && \
    mkdir bcc/build; cd bcc/build && \
    cmake .. \
      -DENABLE_EXAMPLES=OFF \
      -DENABLE_LLVM_SHARED=OFF \
      -DENABLE_MAN=OFF \
      -DENABLE_USDT=ON \
      -DENABLE_TESTS=OFF && \
    make -j$(nproc) bcc-shared && \
    cd src/python/bcc-python3 && \
    pip3 wheel . -w /wheels && \
    cd /wheels && \
    python3 -m zipfile -e $(ls bcc-*.whl | head -n1) /wheels/bcc

FROM docker.io/debian:sid-slim

COPY --from=builder /wheels /wheels
# links are not preserved, but it won't break anything
COPY --from=builder /root/bcc/build/src/cc/libbcc*.a* /root/bcc/build/src/cc/libbcc*.so* /usr/local/lib/

RUN apt-get update && \
    apt-get install -y --no-install-recommends python3 libelf1t64 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    cd /wheels && \
    python3 -m zipfile -e $(ls bcc-*.whl | head -n1) /wheels/bcc && \
    cp -r /wheels/bcc/* /usr/lib/python3/dist-packages/ && \
    rm -rf /wheels

# TODO glibc
# FROM gcr.io/distroless/python3-debian12:debug

# COPY --from=builder /wheels /wheels
# # links are not preserved, but it won't break anything
# COPY --from=builder \
#   /root/bcc/build/src/cc/libbcc*.a* /root/bcc/build/src/cc/libbcc*.so* \
#   /usr/lib/*/libelf*.a* /usr/lib/*/libelf*.so* \
#   /usr/lib/*/libzstd*.a* /usr/lib/*/libzstd*.so* \
#   /usr/local/lib/
# COPY --from=builder /wheels/* /usr/lib/python3/dist-packages/

# ENV LD_LIBRARY_PATH=/usr/local/lib:\$LD_LIBRARY_PATH

# SHELL ["/busybox/sh", "-c"]
