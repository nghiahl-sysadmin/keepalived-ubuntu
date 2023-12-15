FROM alpine:latest

# Variables
ARG keepalived_version=2.2.8

# Install necessary tools
RUN apk update && \
    apk add --no-cache \
    tar \
    curl \
    openssl-dev \
    libnl3-dev \
    libnfnetlink-dev \
    libmnl-dev \
    pkgconf \
    build-base \
    && curl -o /opt/keepalived.tar.gz -SL https://www.keepalived.org/software/keepalived-${keepalived_version}.tar.gz \
    && mkdir -p /opt/keepalived \
    && tar -xzf /opt/keepalived.tar.gz -C /opt/keepalived --strip-components=1 \
    && cd /opt/keepalived \
    && ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
    && make && make install \
    && cd - && mkdir -p /etc/keepalived \
    && rm -rf /opt/keepalived.tar.gz \
    && rm -rf /opt/keepalived \
    && rm -rf /var/cache/apk/* \
    && adduser -S keepalived_script \
    && apk del --no-cache \
    openssl-dev \
    libnl3-dev \
    libnfnetlink-dev \
    libmnl-dev \
	
WORKDIR /etc/keepalived

# Copy sources
COPY docker-entrypoint.sh /

# Add permissions
RUN chmod +x /docker-entrypoint.sh

# Final touches
ENTRYPOINT ["sh", "docker-entrypoint.sh"]
