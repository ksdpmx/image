FROM docker.io/debian:sid-slim AS kubectl

ARG TARGETARCH
# https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux
ARG VERSION="v1.34.1"

SHELL ["/bin/bash", "-c"]
WORKDIR /
RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates curl && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN curl -LO "https://dl.k8s.io/release/${VERSION}/bin/linux/${TARGETARCH}/kubectl" && \
  curl -LO "https://dl.k8s.io/release/${VERSION}/bin/linux/${TARGETARCH}/kubectl.sha256" && \
  echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check && \
  chmod +x kubectl

FROM ghcr.io/ksdpmx/curl:8845b1d AS curl

FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=kubectl /kubectl /usr/local/bin/kubectl
COPY --from=curl /usr/local/bin/curl /usr/local/bin/curl
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    ca-certificates \
    less \
    jq \
    yq && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/usr/local/bin/kubectl"]
