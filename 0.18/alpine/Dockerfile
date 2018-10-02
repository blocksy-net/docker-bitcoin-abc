FROM alpine:3.8

RUN adduser -S bitcoin
RUN apk --no-cache add su-exec
  
ENV BITCOIN_ABC_VERSION=0.18.2
ENV BITCOIN_ABC_DIST=bitcoin-abc-${BITCOIN_ABC_VERSION}-x86_64-linux-gnu.tar.gz
ENV BITCOIN_ABC_SHASUM="28d8511789a126aff16e256a03288948f2660c3c8cb0a4c809c5a8618a519a16  ${BITCOIN_ABC_DIST}"
ENV BITCOIN_ABC_DATA=/home/bitcoin/.bitcoin

RUN wget -O $BITCOIN_ABC_DIST https://download.bitcoinabc.org/${BITCOIN_ABC_VERSION}/linux/${BITCOIN_ABC_DIST} \
&& echo "$BITCOIN_ABC_SHASUM" | sha256sum -c \
&& mkdir bitcoin-bin \
&& tar -xzf $BITCOIN_ABC_DIST -C bitcoin-bin --strip-components=2 --exclude=*-qt \
&& mv bitcoin-bin/bitcoind bitcoin-bin/bitcoin-cli bitcoin-bin/bitcoin-tx /usr/local/bin/ \
&& rm -r bitcoin-bin \
&& rm $BITCOIN_ABC_DIST

COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
