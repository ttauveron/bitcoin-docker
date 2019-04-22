FROM ubuntu:18.04

LABEL maintainer.0="João Fonseca (@joaopaulofonseca)" \
  maintainer.1="Pedro Branco (@pedrobranco)" \
  maintainer.2="Rui Marinho (@ruimarinho)"

RUN useradd -r bitcoin \
  && apt-get update -y \
  && apt-get install -y curl gnupg gosu \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV BITCOIN_VERSION=0.17.1
ENV BITCOIN_DATA=/home/bitcoin/.bitcoin
ENV PATH=/opt/bitcoin-${BITCOIN_VERSION}/bin:$PATH

RUN curl -SL https://bitcoin.org/laanwj-releases.asc | gpg --batch --import \
  && curl -SLO https://bitcoin.org/bin/bitcoin-core-${BITCOIN_VERSION}/SHA256SUMS.asc \
  && curl -SLO https://bitcoin.org/bin/bitcoin-core-${BITCOIN_VERSION}/bitcoin-${BITCOIN_VERSION}-x86_64-linux-gnu.tar.gz \
  && gpg --verify SHA256SUMS.asc \
  && grep " bitcoin-${BITCOIN_VERSION}-x86_64-linux-gnu.tar.gz\$" SHA256SUMS.asc | sha256sum -c - \
  && tar -xzf *.tar.gz -C /opt \
  && rm *.tar.gz *.asc

## Install useful debug tools
RUN apt update && \
	apt install -y bash-completion && \
	curl -SLO https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/bitcoin-cli.bash-completion && \
	curl -SLO https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/bitcoin-tx.bash-completion && \
	curl -SLO https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/bitcoind.bash-completion && \
	mkdir /etc/bash-completion.d/ && \
	mv *.bash-completion /etc/bash-completion.d/ && \
	echo "source /etc/profile.d/bash_completion.sh" >> /etc/profile && \
	echo "source /etc/bash-completion.d/*" >> /etc/profile


COPY docker-entrypoint.sh /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18443 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
