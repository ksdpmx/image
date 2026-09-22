# 2026-09-22T16:23:38+0800
FROM golang:1.26 AS builder
ARG VERSION="4e860fc3cfeed83709ddbd560c35c3a31c7f3896"

WORKDIR /workspace
RUN git clone -b ${VERSION} https://github.com/ksdpmx/flux-mirror && \
    cd ./flux-mirror && \
    CGO_ENABLED=0 go build -ldflags="-s -w" -a -o flux-mirror ./cmd/flux-mirror/

FROM gcr.io/distroless/static-debian13:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /workspace/flux-mirror/flux-mirror /usr/local/bin/flux-mirror

ENTRYPOINT ["/usr/local/bin/flux-mirror"]
