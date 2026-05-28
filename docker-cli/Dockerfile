# 2026-05-28T01:45:12+0800
# https://github.com/docker-library/docker/blob/bc9e03bd1093d09d4aa4c3d74ca0a1d42acdc3e8/29/cli/Dockerfile

FROM debian:stable-slim
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY
ENV DEBIAN_FRONTEND=noninteractive

ENV DOCKER_VERSION=29.5.2
ENV DOCKER_BUILDX_VERSION=0.34.1
ENV DOCKER_COMPOSE_VERSION=5.1.4

ENV DOCKER_TLS_CERTDIR=/certs

COPY modprobe.sh /usr/local/bin/modprobe
COPY docker-entrypoint.sh /usr/local/bin/

RUN set -eux; \
    apt-get update; \
    # so far we don't use ssh and git in docker
    # apt-get install -y --no-install-recommends ca-certificates openssh-client git && \
    apt-get install -y --no-install-recommends ca-certificates curl; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "$dpkgArch" in \
      'amd64') \
        dockerArch='x86_64'; \
        dockerBuildxArch='amd64'; \
        dockerBuildxSha256='f1332ddb9010bd0b72628266c3a906d9a6979848033df4c8d9bd2cd113bae12b'; \
        dockerComposeArch='x86_64'; \
        dockerComposeSha256='33b208d7e76639db742fae84b966cc01dacae58ca3fc4dabbc907045aefdf0c4'; \
        ;; \
      'arm64') \
        dockerArch='aarch64'; \
        dockerBuildxArch='arm64'; \
        dockerBuildxSha256='c34e32dd6ea2653d960d6c099c9f09b9077e4a37504d2d31e5066eccc3904231'; \
        dockerComposeArch='aarch64'; \
        dockerComposeSha256='d4fb48b72857810314d3ee77123c89954101844efa4788031221f4c370495946'; \
        ;; \
      *) echo >&2 "error: unsupported architecture: $dpkgArch"; exit 1 ;; \
    esac; \
    dockerUrl="https://download.docker.com/linux/static/stable/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; \
    curl -fsSL "$dockerUrl" | tar --extract \
      --gzip \
      --strip-components 1 \
      --directory /usr/local/bin/ \
      --no-same-owner \
      'docker/docker'; \
    docker --version; \
    dockerBuildxUrl="https://github.com/docker/buildx/releases/download/v${DOCKER_BUILDX_VERSION}/buildx-v${DOCKER_BUILDX_VERSION}.linux-${dockerBuildxArch}"; \
    curl -fsSL -o 'docker-buildx' "$dockerBuildxUrl"; \
    echo "$dockerBuildxSha256 *"'docker-buildx' | sha256sum -c -; \
    dockerBuildxBinary='/usr/local/libexec/docker/cli-plugins/docker-buildx'; \
    mkdir -p "$(dirname "$dockerBuildxBinary")"; \
    mv -vT 'docker-buildx' "$dockerBuildxBinary"; \
    chmod +x "$dockerBuildxBinary"; \
    dockerComposeUrl="https://github.com/docker/compose/releases/download/v${DOCKER_COMPOSE_VERSION}/docker-compose-linux-${dockerComposeArch}"; \
    curl -fsSL -o 'docker-compose' "$dockerComposeUrl"; \
    echo "$dockerComposeSha256 *"'docker-compose' | sha256sum -c -; \
    dockerComposeBinary='/usr/local/libexec/docker/cli-plugins/docker-compose'; \
    mkdir -p "$(dirname "$dockerComposeBinary")"; \
    mv -vT 'docker-compose' "$dockerComposeBinary"; \
    chmod +x "$dockerComposeBinary"; \
    ln -sv "$dockerComposeBinary" /usr/local/bin/; \
    apt-get purge -y --auto-remove curl; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*; \
    [ -e /etc/nsswitch.conf ] && grep -E '^hosts:.*files.*dns' /etc/nsswitch.conf; \
     groupadd --gid 2375 docker; \
    mkdir /certs /certs/client && chmod 1777 /certs /certs/client; \
    chmod +x /usr/local/bin/docker-entrypoint.sh /usr/local/bin/modprobe

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["sh"]
