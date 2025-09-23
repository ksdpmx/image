FROM --platform=${BUILDPLATFORM} docker.io/debian:sid-slim AS builder

ARG TARGETARCH
ARG VERSION="v1.34.0"
ARG SHA256SUM_AMD64="a8ff2a3edb37a98daf3aba7c3b284fe0aa5bff24166d896ab9ef64c8913c9f51"
ARG SHA256SUM_ARM64="c31d252e203df5f4cf37f314bd3092eb79087e791631c1e607087c74b6d0423f"

WORKDIR /
RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates wget && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN curl -LO https://github.com/kubernetes-sigs/cri-tools/releases/download/${VERSION}/crictl-${VERSION}-linux-${TARGETARCH}.tar.gz && \
  if [[ ${TARGETARCH} == "amd64" ]]; then echo "${SHA256SUM_AMD64}  crictl-${VERSION}-linux-${TARGETARCH}.tar.gz"; else echo "${SHA256SUM_ARM64}  crictl-${VERSION}-linux-${TARGETARCH}.tar.gz"; fi && \
  | sha256sum -c - && \
  tar zxvf crictl-$VERSION-linux-${TARGETARCH}.tar.gz

FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /crictl /usr/local/bin/crictl
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    less \
    ripgrep \
    file \
    jq \
    yq \
    strace \
    lsof \
    iptables \
    nftables \
    iproute2 \
    ipset \
    bridge-utils \
    ethtool \
    iputils-ping \
    procps \
    ncat \
    tcpdump \
    traceroute \
    conntrack && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/bin/bash"]
