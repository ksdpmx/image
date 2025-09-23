FROM --platform=${BUILDPLATFORM} docker.io/debian:sid-slim AS builder

ARG TARGETARCH

WORKDIR /
RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates curl && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN curl -LO "https://dl.k8s.io/release/v1.34.1/bin/linux/${TARGETARCH}/kubectl" && \
  curl -LO "https://dl.k8s.io/release/v1.34.1/bin/linux/${TARGETARCH}/kubectl.sha256" && \
  echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check && \
  chmod +x kubectl

FROM docker.io/debian:sid-slim

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /kubectl /usr/local/bin/kubectl
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    less \
    jq \
    yq && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENTRYPOINT [ "/usr/local/bin/kubectl" ]
