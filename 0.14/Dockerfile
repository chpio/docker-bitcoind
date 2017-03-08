FROM chpio/gosu:jessie

RUN { \
	set -ex; \

	BITCOIND_VERSION="0.14.0"; \

	# add user & group
	groupadd --system --gid 1000 bitcoind; \
	useradd --system --uid 1000 --gid 1000 bitcoind; \

	BUILD_DEPS="wget ca-certificates"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \

	# download
	wget \
		"https://bitcoin.org/bin/bitcoin-core-${BITCOIND_VERSION}/bitcoin-${BITCOIND_VERSION}-x86_64-linux-gnu.tar.gz" \
		--output-document="bitcoind.tar.gz"; \

	# install
	tar -xzf bitcoind.tar.gz; \
	mv bitcoin-${BITCOIND_VERSION}/lib/* /usr/lib; \
	mv bitcoin-${BITCOIND_VERSION}/bin/bitcoind /usr/bin; \

	# clean up
	apt-get purge -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
}

VOLUME /data/bitcoind

COPY bitcoind.conf /etc/

COPY start-bitcoind /usr/bin/
ENTRYPOINT ["start-bitcoind"]
