# https://hub.docker.com/r/nghiahl/keepalived
FROM ubuntu:focal

# Variables
ARG KEEPALIVED_URL=https://keepalived.org/software/keepalived-2.2.8.tar.gz
ARG KEEPALIVED_MD5=8c26f75a8767e5341d82696e1e717115

RUN useradd --no-create-home keepalived_script

# Install extensions
RUN set -eux; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get -y update && apt-get -y install --no-install-recommends \
	ca-certificates \
	tar \
	make \
	url \
	libssl-dev \
	libnl-3-dev \
	libnl-genl-3-dev \
	libnfnetlink-dev \
	libmnl-dev \
	pkg-config \
	build-essential \
	; \
	curl -o /opt/keepalived.tar.gz -SL "$KEEPALIVED_URL"; \
	echo "$KEEPALIVED_MD5 /opt/keepalived.tar.gz" | md5sum -c; \
	mkdir -p /opt/keepalived; \
	tar -xzf /opt/keepalived.tar.gz -C /opt/keepalived --strip-components=1; \
	cd /opt/keepalived; \
	./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var; \
	make && make install; \
	cd - && mkdir -p /etc/keepalived; \
	rm -rf /opt/keepalived.tar.gz; \
	rm -rf /opt/keepalived; \
	rm -rf /var/lib/apt/lists/* \
	; \
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	find /usr/local -type f -executable -exec ldd '{}' ';' \
	@@ -43,13 +43,13 @@ RUN set -eux; \
		| xargs -r dpkg-query --search \
		| cut -d: -f1 \
		| sort -u \
		| grep -v -e 'libnl-3-200:amd64' -e 'libnl-genl-3-200:amd64' \
		| xargs -r apt-mark manual \
	; \
	apt-mark hold libnl-3-200 \
	libnl-genl-3-200 \
	curl \
	; \
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false

WORKDIR /etc/keepalived

# Copy sources
COPY docker-entrypoint.sh /usr/local/bin/

# Add permissions
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Final touches
ENTRYPOINT ["docker-entrypoint.sh"]
