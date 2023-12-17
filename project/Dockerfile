# https://hub.docker.com/r/nghiahl/keepalived
FROM ubuntu:latest

# Variables
ARG keepalived_version=2.2.8

RUN set -eux; \
	groupadd --gid 99 --system keepalived; \
	useradd \
		--gid keepalived \
		--system \
		--uid 99 \
		keepalived \
	; \
	mkdir /etc/keepalived; \
	chown keepalived:keepalived /etc/keepalived
 
# Install extensions
RUN set -eux; \
        \
        savedAptMark="$(apt-mark showmanual)"; \
        apt-get -y update && apt-get -y install --no-install-recommends \
		   ca-certificates \
                   tar \
                   curl \
		   make \
     		   wget \
                   libssl-dev \
                   libnl-3-dev \
                   libnl-genl-3-dev \
                   libnfnetlink-dev \
                   libmnl-dev \
                   pkg-config \
                   build-essential \
        ; \
    	curl -o /opt/keepalived.tar.gz -SL https://www.keepalived.org/software/keepalived-${keepalived_version}.tar.gz; \
    	mkdir -p /opt/keepalived; \
    	tar -xzf /opt/keepalived.tar.gz -C /opt/keepalived --strip-components=1; \
    	cd /opt/keepalived; \
    	./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var; \
    	make && make install; \
    	cd - && mkdir -p /etc/keepalived; \
    	rm -rf /opt/keepalived.tar.gz; \
    	rm -rf /opt/keepalived; \
     	rm -rf /var/lib/apt/lists/*; \
	\
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
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false

WORKDIR /etc/keepalived

# Copy sources
COPY docker-entrypoint.sh /usr/local/bin/

# Add permissions
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

USER keepalived

# Final touches
ENTRYPOINT ["docker-entrypoint.sh"]
