FROM chpio/gosu:jessie

RUN { \
	set -ex; \

	BITCOIN_VERSION="0.12.0"; \

	# add user & group
	groupadd --system --gid 1000 bitcoin; \
	useradd --system --uid 1000 --gid 1000 bitcoin; \

	BUILD_DEPS="wget ca-certificates"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \

	# download
	wget \
		"https://bitcoin.org/bin/bitcoin-core-${BITCOIN_VERSION}/bitcoin-${BITCOIN_VERSION}-linux64.tar.gz" \
		--output-document="bitcoin.tar.gz"; \

	# install
	tar -xzf bitcoin.tar.gz; \
	mv bitcoin-${BITCOIN_VERSION}/lib/* /usr/lib; \
	mv bitcoin-${BITCOIN_VERSION}/bin/bitcoind /usr/bin; \

	# clean up
	apt-get purge -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
}

VOLUME /data/bitcoin

COPY bitcoin.conf /etc/

COPY start-bitcoin /usr/bin/
ENTRYPOINT ["start-bitcoin"]
