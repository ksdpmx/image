FROM golang:1.26 AS builder
ARG VERSION="v3.6.10"

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/etcd-io/etcd && \
    cd ./etcd/etcdctl && \
    CGO_ENABLED=0 go build -a -trimpath -ldflags="-s -w" -o etcdctl

FROM gcr.io/distroless/static-debian13:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /workspace/etcd/etcdctl/etcdctl /usr/local/bin/etcdctl

ENTRYPOINT ["/usr/local/bin/etcdctl"]
