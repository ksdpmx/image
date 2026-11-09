# 2026-11-09T15:57:17+0800
FROM alpine:3 AS builder

ARG TARGETARCH
ARG WIZCLI_VERSION="1.83.0"

SHELL ["/bin/sh", "-ec"]
WORKDIR /tmp

RUN apk add --no-cache ca-certificates curl tar && \
    case "${TARGETARCH:-amd64}" in \
      amd64|x86_64) WIZ_ARCH="amd64" ;; \
      arm64|aarch64) WIZ_ARCH="arm64" ;; \
      *) echo "Unsupported TARGETARCH: ${TARGETARCH:-amd64}" && exit 1 ;; \
    esac && \
    curl -fsSL "https://downloads.wiz.io/v1/wizcli/${WIZCLI_VERSION}/wizcli-linux-${WIZ_ARCH}" -o /tmp/wizcli && \
    chmod 0755 /tmp/wizcli

FROM gcr.io/distroless/static-debian13:debug

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

COPY --from=builder /tmp/wizcli /usr/local/bin/wizcli

ENTRYPOINT ["/usr/local/bin/wizcli"]
CMD ["--help"]
