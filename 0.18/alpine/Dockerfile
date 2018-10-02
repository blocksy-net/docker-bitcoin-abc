FROM alpine:3.8

RUN adduser -S bitcoin
RUN apk --no-cache add \
  boost \
  boost-program_options \
  curl \
  libevent \
  libressl \
  libzmq \
  su-exec
  
ENV BITCOIN_ABC_VERSION=0.18.2
ENV BITCOIN_ABC_PREFIX=bitcoin-abc-${BITCOIN_ABC_VERSION}-x86_64-linux-gnu
ENV BITCOIN_ABC_SHASUM="28d8511789a126aff16e256a03288948f2660c3c8cb0a4c809c5a8618a519a16  {BITCOIN_ABC_PREFIX}.tar.gz"
ENV BITCOIN_ABC_DATA=/home/bitcoin/.bitcoin
ENV PATH=/opt/${BITCOIN_ABC_PREFIX}/bin:$PATH

RUN wget https://download.bitcoinabc.org/${BITCOIN_ABC_VERSION}/linux/{BITCOIN_ABC_PREFIX}.tar.gz
RUN echo "${BITCOIN_ABC_SHASUM}" | sha256sum -c
RUN tar -xzf *.tar.gz

COPY --from=bitcoin-abc /opt /opt
COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
