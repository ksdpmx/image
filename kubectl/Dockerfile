FROM --platform=${BUILDPLATFORM} docker.io/debian:sid-slim AS builder

WORKDIR /
RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates curl && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*
RUN curl -LO "https://dl.k8s.io/release/v1.34.1/bin/linux/amd64/kubectl" && \
  curl -LO "https://dl.k8s.io/release/v1.34.1/bin/linux/amd64/kubectl.sha256" && \
  echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check && \
  exit 0

FROM --platform=${BUILDPLATFORM} docker.io/debian:sid-slim
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
