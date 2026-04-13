FROM ghcr.io/ksdpmx/basebuilder:a806384 AS builder
ARG VERSION="v0.25.1"
ARG COMMIT="e491811e5d648288c01f42ce087967b271f504a0"
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      asciidoctor \
      bpftool \
      binutils-dev \
      bison \
      build-essential \
      clang \
      cmake \
      flex \
      libbpf-dev \
      libbpfcc-dev \
      libcereal-dev \
      libdw-dev \
      libelf-dev \
      libiberty-dev \
      libpcap-dev \
      llvm-dev \
      libclang-dev \
      libclang-cpp-dev \
      pahole \
      xxd \
      zip \
      zlib1g-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/bpftrace/bpftrace && \
    cd ./bpftrace && \
    git reset --hard ${COMMIT} && \
    cmake -B ./build -DBUILD_TESTING=OFF -DCMAKE_BUILD_TYPE=Release -DUSE_SYSTEM_LIBBPF=On && \
    make -C ./build -j$(nproc)

FROM debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      libbinutils \
      libbpfcc \
      libdw1t64 \
      libclang1-21 \
      libclang-cpp21 \
      libllvm21 \
      zlib1g \
      libelf1 \
      libpcap0.8t64 \
      libstdc++6 \
      libc6 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /workspace/bpftrace/build/src/bpftrace /app/bpftrace

ENTRYPOINT ["/app/bpftrace"]
