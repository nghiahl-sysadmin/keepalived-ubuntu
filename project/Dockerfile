# https://hub.docker.com/r/nghiahl/keepalived
FROM ubuntu:latest

# Variables
ARG keepalived_version=2.2.8

# Install extensions
RUN apt-get -y update && apt-get -y install \
    tar \
    curl \
    libssl-dev \
    libnl-3-dev \
    libnl-genl-3-dev \
    libnfnetlink-dev \
    libmnl-dev \
    pkg-config \
    build-essential \
    && curl -o /opt/keepalived.tar.gz -SL https://www.keepalived.org/software/keepalived-${keepalived_version}.tar.gz \
    && mkdir -p /opt/keepalived \
    && tar -xzf /opt/keepalived.tar.gz -C /opt/keepalived --strip-components=1 \
    && cd /opt/keepalived \
    && ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
    && make && make install \
    && cd - && mkdir -p /etc/keepalived \
    && rm -rf /opt/keepalived.tar.gz \
    && rm -rf /opt/keepalived \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && useradd --no-create-home keepalived_script

WORKDIR /etc/keepalived

# Copy sources
COPY docker-entrypoint.sh /usr/local/bin/

# Add permissions
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Final touches
ENTRYPOINT ["docker-entrypoint.sh"]
