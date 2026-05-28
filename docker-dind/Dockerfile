# 2026-05-28T12:35:06+0800
# https://github.com/docker-library/docker/blob/bc9e03bd1093d09d4aa4c3d74ca0a1d42acdc3e8/29/dind/Dockerfile
FROM ghcr.io/ksdpmx/docker-cli:5bd7f5c
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY
ENV DEBIAN_FRONTEND=noninteractive

ENV DOCKER_VERSION=29.5.2
ENV DIND_COMMIT=8d9e3502aba39127e4d12196dae16d306f76993d

COPY dockerd-entrypoint.sh /usr/local/bin/

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
      btrfs-progs \
      curl \
      e2fsprogs \
      iptables \
      openssl \
      pigz \
      uidmap \
      xfsprogs \
      xz-utils; \
    groupadd --system dockremap; \
    useradd --system --gid dockremap --no-create-home --home-dir /nonexistent --shell /usr/sbin/nologin dockremap; \
    echo 'dockremap:165536:65536' >> /etc/subuid; \
    echo 'dockremap:165536:65536' >> /etc/subgid; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "$dpkgArch" in \
      'amd64') \
        dockerArch='x86_64'; \
        ;; \
      'arm64') \
        dockerArch='aarch64'; \
        ;; \
      *) echo >&2 "error: unsupported architecture: $dpkgArch"; exit 1 ;; \
    esac; \
    dockerUrl="https://download.docker.com/linux/static/stable/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; \
    curl -fsSL "$dockerUrl" | tar --extract \
      --gzip \
      --strip-components 1 \
      --directory /usr/local/bin/ \
      --no-same-owner \
      --exclude 'docker/docker'; \
    curl -fsSL -o '/usr/local/bin/dind' "https://raw.githubusercontent.com/docker/docker/${DIND_COMMIT}/hack/dind"; \
    chmod +x /usr/local/bin/dind; \
    chmod +x /usr/local/bin/dockerd-entrypoint.sh; \
    apt-get purge -y --auto-remove curl; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*

VOLUME /var/lib/docker
EXPOSE 2375 2376

ENTRYPOINT ["dockerd-entrypoint.sh"]
CMD []
