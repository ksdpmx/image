FROM golang:1.23 AS builder

WORKDIR /workspace
RUN git clone https://github.com/ksdpmx/go-httpbin && \
    cd ./go-httpbin && \
    CGO_ENABLED=0 go build -ldflags="-s -w" -o ./go-httpbin ./cmd/go-httpbin

FROM gcr.io/distroless/static:nonroot

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /workspace/go-httpbin /bin

EXPOSE 8080
CMD ["/bin/go-httpbin"]
