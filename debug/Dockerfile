FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

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
