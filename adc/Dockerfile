FROM node:lts-bookworm-slim AS builder
ARG VERSION="feature/multi-owner"
ARG COMMIT="492948a"

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
ENV NX_DAEMON="false"

WORKDIR /workspace
RUN apt update && \
    apt install -y git && \
    git clone -b ${VERSION} --depth 1 https://github.com/ksdpmx/adc && \
    cd ./adc && \
    git reset --hard ${COMMIT} && \
    corepack enable && \
    pnpm install nx -g && \
    pnpm install && \
    NODE_ENV=production nx build cli

FROM node:lts-bookworm-slim

COPY --from=builder /workspace/adc/dist/apps/cli/main.js .

ENTRYPOINT [ "node", "main.js" ]
