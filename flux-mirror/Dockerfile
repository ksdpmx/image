# 2026-09-22T14:31:16+0800
FROM golang:1.26 AS builder
ARG VERSION="feat/legacy-cosign-signature-fallback"

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/ksdpmx/flux-mirror && \
    cd ./flux-mirror && \
    CGO_ENABLED=0 go build -ldflags="-s -w" -a -o flux-mirror ./cmd/flux-mirror/

FROM gcr.io/distroless/static-debian13:debug

LABEL org.opencontainers.image.source "https://github.com/ksdpmx/image"

COPY --from=builder /workspace/flux-mirror/flux-mirror /usr/local/bin/flux-mirror

ENTRYPOINT ["/usr/local/bin/flux-mirror"]
