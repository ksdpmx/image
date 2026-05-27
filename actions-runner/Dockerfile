# 2026-05-28T00:16:09+0800
FROM ghcr.io/actions/actions-runner:2.334.0

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends iptables && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER runner
