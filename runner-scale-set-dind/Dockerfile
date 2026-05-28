# 2026-05-28T13:55:58+0800

FROM ghcr.io/ksdpmx/docker-dind:de06881
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY
ENV DEBIAN_FRONTEND=noninteractive

ENV NODE_VERSION=24.11.1

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends libcap2-bin curl; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "$dpkgArch" in \
      'amd64') \
        nodeArch='x64'; \
        ;; \
      'arm64') \
        nodeArch='arm64'; \
        ;; \
      *) echo >&2 "error: unsupported architecture: $dpkgArch"; exit 1 ;; \
    esac; \
    curl -fsSL "https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-${nodeArch}.tar.xz" \
      | tar -xJ -C /usr/local --strip-components=1; \
    npm install -g @github/copilot; \
    ln -sf "$(command -v copilot)" /usr/local/bin/copilot; \
    apt-get purge -y --auto-remove curl; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*
