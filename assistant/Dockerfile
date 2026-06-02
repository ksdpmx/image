# 2026-06-02T21:59:19+0800
FROM ghcr.io/ksdpmx/basebuilder:c7ca954 AS builder
ARG HTTPS_PROXY
ARG COMMIT="fd14fc9930403874d4c5842ef53d8e53c365eb18"

ENV HTTPS_PROXY=$HTTPS_PROXY

WORKDIR /

RUN git clone https://github.com/ksdpmx/assistant.git && \
    cd ./assistant && \
    git reset --hard ${COMMIT}

FROM ghcr.io/astral-sh/uv:python3.14-trixie
ARG HTTPS_PROXY

ENV HTTPS_PROXY=$HTTPS_PROXY
ENV PYTHONUNBUFFERED=1
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

WORKDIR /app

COPY --from=builder /assistant/pyproject.toml ./
COPY --from=builder /assistant/agent.py ./
COPY --from=builder /assistant/uv.lock ./
RUN uv sync --frozen --no-dev --no-install-project

CMD ["uv", "run", "python", "agent.py"]
