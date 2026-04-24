FROM docker.io/debian:sid-slim AS crictl

ARG TARGETARCH
# https://github.com/kubernetes-sigs/cri-tools?tab=readme-ov-file#install
ARG VERSION="v1.35.0"
ARG SHA256SUM_AMD64="2e141e5b22cb189c40365a11807d69b76b9b3caced89fac2f4ec879408ce2177"
ARG SHA256SUM_ARM64="519071de89b64c43e2a1661bb5489c6c3fd5e9e5fcef75e50e542b0c891f1118"

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

FROM ghcr.io/ksdpmx/curl:69c5861 AS curl

FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=crictl /crictl /usr/local/bin/crictl
COPY --from=curl /usr/local/bin/curl /usr/local/bin/curl
ENV DEBIAN_FRONTEND=noninteractive
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
    traceroute \
    conntrack \
    bind9-dnsutils \
    tshark && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/bin/bash"]
