# 2026-05-28T00:16:09+0800
FROM ghcr.io/actions/actions-runner:2.334.0

ENV NODE_VERSION=24.11.1
ENV COPILOT_VERSION=1.0.52

USER root

RUN set -eux; \
    install -m 0755 -d /etc/apt/keyrings; \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc; \
    chmod a+r /etc/apt/keyrings/docker.asc; \
    . /etc/os-release; \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu ${VERSION_CODENAME} stable" > /etc/apt/sources.list.d/docker.list; \
    apt-get update; \
    apt-get install -y --no-install-recommends iptables docker-ce-cli docker-compose-plugin curl; \
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
#    npm install -g @github/copilot@1.0.52; \
    apt-get purge -y --auto-remove curl; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*

USER runner
