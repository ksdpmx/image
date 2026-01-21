# Copyright the Velero contributors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM --platform=$BUILDPLATFORM golang:1.24-bookworm AS build

ARG TARGETOS
ARG TARGETARCH
ARG TARGETVARIANT
ARG GOPROXY

ENV GOOS=${TARGETOS} \
    GOARCH=${TARGETARCH} \
    GOARM=${TARGETVARIANT} \
    GOPROXY=${GOPROXY}

ARG GITCOMMIT="b72403c"

#COPY . /go/src/velero-plugin-for-aws
WORKDIR /go/src
RUN export GOARM=$( echo "${GOARM}" | cut -c2-) && \
    git clone -b feature/gp3 https://github.com/ksdpmx/velero-plugin-for-aws && \
    cd ./velero-plugin-for-aws && \
    git checkout ${GITCOMMIT} && \
    CGO_ENABLED=0 go build -v -o /go/bin/velero-plugin-for-aws ./velero-plugin-for-aws && \
    CGO_ENABLED=0 go build -v -o /go/bin/cp-plugin ./hack/cp-plugin
FROM scratch
LABEL org.opencontainers.image.source="https://github.com/vmware-tanzu/velero-plugin-for-aws"
COPY --from=build /go/bin/velero-plugin-for-aws /plugins/
COPY --from=build /go/bin/cp-plugin /bin/cp-plugin
USER 65532:65532
ENTRYPOINT ["cp-plugin", "/plugins/velero-plugin-for-aws", "/target/velero-plugin-for-aws"]
