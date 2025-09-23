FROM docker.io/debian:sid-slim AS crictl

ARG TARGETARCH
# https://github.com/kubernetes-sigs/cri-tools?tab=readme-ov-file#install
ARG VERSION="v1.34.0"
ARG SHA256SUM_AMD64="a8ff2a3edb37a98daf3aba7c3b284fe0aa5bff24166d896ab9ef64c8913c9f51"
ARG SHA256SUM_ARM64="c31d252e203df5f4cf37f314bd3092eb79087e791631c1e607087c74b6d0423f"

SHELL ["/bin/bash", "-c"]
WORKDIR /
RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates curl && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN curl -LO "https://github.com/kubernetes-sigs/cri-tools/releases/download/${VERSION}/crictl-${VERSION}-linux-${TARGETARCH}.tar.gz" && \
  if [[ "${TARGETARCH}" == "amd64" ]]; then echo "${SHA256SUM_AMD64}  crictl-${VERSION}-linux-${TARGETARCH}.tar.gz"; else echo "${SHA256SUM_ARM64}  crictl-${VERSION}-linux-${TARGETARCH}.tar.gz"; fi \
  | sha256sum -c - && \
  tar zxvf crictl-$VERSION-linux-${TARGETARCH}.tar.gz

FROM ghcr.io/ksdpmx/curl:6af1a00 AS curl

FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=crictl /crictl /usr/local/bin/crictl
COPY --from=curl /usr/local/bin/curl /usr/local/bin/curl
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    ca-certificates \
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
