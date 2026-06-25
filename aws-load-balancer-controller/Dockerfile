FROM golang:1.24.5 AS builder
ARG VERSION="release-2.13"
ARG COMMIT="75964470"
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/ksdpmx/aws-load-balancer-controller && \
    cd ./aws-load-balancer-controller && \
    git reset --hard ${COMMIT} && \
    GIT_VERSION=$(git describe --tags --dirty --always) && \
    GIT_COMMIT=$(git rev-parse HEAD) && \
    BUILD_DATE=$(date +%Y-%m-%dT%H:%M:%S%z) && \
    CGO_CPPFLAGS="-D_FORTIFY_SOURCE=2" \
    CGO_LDFLAGS="-Wl,-z,relro,-z,now" \
    go build -buildmode=pie -tags 'osusergo,netgo,static_build' -ldflags="-s -w -linkmode=external -extldflags '-static-pie' -X ${VERSION_PKG}.GitVersion=${GIT_VERSION} -X ${VERSION_PKG}.GitCommit=${GIT_COMMIT} -X ${VERSION_PKG}.BuildDate=${BUILD_DATE}" -a -o /controller main.go

FROM gcr.io/distroless/static-debian13:debug-nonroot

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

ENV VERSION_PKG=sigs.k8s.io/aws-load-balancer-controller/pkg/version

COPY --from=builder /controller /controller

ENTRYPOINT ["/controller"]
