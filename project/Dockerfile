# https://hub.docker.com/r/nghiahl/keepalived
FROM ubuntu:focal

# Variables
ARG KEEPALIVED_URL=https://www.keepalived.org/software/keepalived-2.3.1.tar.gz
ARG KEEPALIVED_MD5=5434ebc3becb6f86ebc3adc04e3e7e6d

RUN useradd --system --no-create-home keepalived_script
 
# Install extensions
RUN set -eux; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	apt-get -y update; \
 	DEBIAN_FRONTEND=noninteractive apt-get -y install --no-install-recommends \
	ca-certificates \
	tar \
	make \
	curl \
	libssl-dev \
	libnl-3-dev \
	libnl-genl-3-dev \
	libnfnetlink-dev \
	libmnl-dev \
	pkg-config \
	build-essential \
        libxtables-dev \
        libip4tc-dev \
        libip6tc-dev \
        libipset-dev \
        libmagic-dev \
        libsnmp-dev \
        libglib2.0-dev \
        libpcre2-dev \
        libkmod-dev \
	; \
	curl -o /usr/src/keepalived.tar.gz -SL "$KEEPALIVED_URL"; \
	echo "$KEEPALIVED_MD5 /usr/src/keepalived.tar.gz" | md5sum -c; \
	mkdir -p /usr/src/keepalived; \
	tar -xzf /usr/src/keepalived.tar.gz -C /usr/src/keepalived --strip-components=1; \
	cd /usr/src/keepalived; \
	./configure --enable-snmp-rfc --enable-snmp --enable-snmp-vrrp --enable-dbus --enable-dbus-create-instance --enable-asserts --enable-bfd --enable-regex --enable-regex-timers --enable-timer-check --enable-log-file --prefix=/usr/local --sysconfdir=/etc --localstatedir=/var;\
	\
	make && make install;\
	cd - && mkdir -p /etc/keepalived;\
	rm -rf /usr/src/keepalived.tar.gz; \
	rm -rf /usr/src/keepalived; \
	rm -rf /var/lib/apt/lists/* \
	; \
	apt-mark auto '.*' > /dev/null; \
	[ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; \
	find /usr/local -type f -executable -exec ldd '{}' ';' \
		| awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); print so }' \
		| sort -u \
		| xargs -r dpkg-query --search \
		| cut -d: -f1 \
		| sort -u \
		| xargs -r apt-mark manual \
	; \
	apt-mark hold \
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
