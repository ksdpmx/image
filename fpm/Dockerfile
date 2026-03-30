FROM ghcr.io/ksdpmx/basebuilder:f501048

LABEL org.opencontainers.image.source="https://github.com/ksdpmx/image"

RUN DEBIAN_FRONTEND=noninteractive apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y ruby ruby-dev rubygems rpm \
    && gem install dotenv -v 2.8.1 \
    && gem install fpm

CMD /usr/local/bin/fpm
