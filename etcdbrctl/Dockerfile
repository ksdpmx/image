# SPDX-FileCopyrightText: 2024 SAP SE or an SAP affiliate company and Gardener contributors
#
# SPDX-License-Identifier: Apache-2.0

FROM golang:1.25.7 AS builder
ARG VERSION="v0.42.0"

WORKDIR /workspace
RUN git clone -b ${VERSION} --depth 1 https://github.com/gardener/etcd-backup-restore && \
    cd ./etcd-backup-restore && \
    make build

FROM gcr.io/distroless/static-debian12:debug-nonroot

COPY --from=builder /workspace/etcd-backup-restore/bin/etcdbrctl /etcdbrctl
WORKDIR /
ENTRYPOINT ["/etcdbrctl"]
