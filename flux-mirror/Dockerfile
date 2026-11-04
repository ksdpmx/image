# 2026-09-22T16:23:38+0800
FROM golang:1.27 AS builder
ARG VERSION="24d1caae7b7b7fa5dc7fa461acd14976019a870f"

WORKDIR /workspace
RUN git clone https://github.com/ksdpmx/flux-mirror && \
    cd ./flux-mirror && \
    git reset --hard ${VERSION} && \
    CGO_ENABLED=0 go build -ldflags="-s -w" -a -o flux-mirror ./cmd/flux-mirror/

FROM gcr.io/distroless/static-debian13:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /workspace/flux-mirror/flux-mirror /usr/local/bin/flux-mirror

ENTRYPOINT ["/usr/local/bin/flux-mirror"]
