FROM chpio/gosu:stretch

RUN { \
	set -ex; \

	BITCOIND_VERSION="0.15.0.1"; \

	# add user & group
	groupadd --system --gid 1000 bitcoind; \
	useradd --system --uid 1000 --gid 1000 bitcoind; \

	BUILD_DEPS="wget ca-certificates"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \

	# download
	mkdir /tmp/bitcoind; \
	cd /tmp/bitcoind; \
	wget \
		--quiet \
		--output-document="-" \
		"https://bitcoin.org/bin/bitcoin-core-${BITCOIND_VERSION}/bitcoin-${BITCOIND_VERSION}-x86_64-linux-gnu.tar.gz" \
	| tar \
		--extract \
		--gzip \
		--strip-components="1"; \

	# install
	mv lib/* /usr/local/lib; \
	mv bin/bitcoind /usr/local/bin; \

	# clean up
	apt-get purge -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
}

VOLUME /data/bitcoind

COPY bitcoind.conf /etc/

COPY start-bitcoind /usr/bin/
ENTRYPOINT ["start-bitcoind"]
