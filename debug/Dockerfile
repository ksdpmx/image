FROM --platform=${BUILDPLATFORM} docker.io/debian:sid-slim

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
