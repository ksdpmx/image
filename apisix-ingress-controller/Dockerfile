FROM golang:1.26 AS builder
ARG VERSION="feature/multi-owner"
ARG COMMIT="1162d03"

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/ksdpmx/apisix-ingress-controller && \
    git reset --hard ${COMMIT} && \
    cd ./apisix-ingress-controller && \
    CGO_ENABLED=0 go build -a -trimpath -ldflags="-s -w" -o ./bin/apisix-ingress-controller ./cmd/main.go

FROM gcr.io/distroless/static-debian12:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /workspace/apisix-ingress-controller/bin/apisix-ingress-controller /app/apisix-ingress-controller

ENTRYPOINT ["/app/apisix-ingress-controller"]
CMD ["-c", "/app/conf/config.yaml"]
