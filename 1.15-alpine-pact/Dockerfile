FROM merit/golang-ci:1.15

WORKDIR ../

ENV PACT_VERSION=1.88.77 \
    GLIBC_VERSION=2.33-r0

# Glibc for alpine. Pact has a dependancie on glibc in its ruby implementation, musl-dev from base image is not compatible
# https://github.com/pact-foundation/pact-ruby-standalone/wiki/Using-the-pact-ruby-standalone-with-Alpine-Linux-Docker
RUN apk --no-cache add bash \
    && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
    && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
    && apk add glibc-${GLIBC_VERSION}.apk

# Pact binaries and related dependencies
RUN wget https://github.com/pact-foundation/pact-ruby-standalone/releases/download/v${PACT_VERSION}/pact-${PACT_VERSION}-linux-x86_64.tar.gz \
    && tar xzf pact-${PACT_VERSION}-linux-x86_64.tar.gz \
    && rm -rf pact-${PACT_VERSION}-linux-x86_64.tar.gz \
    && ln -s /pact/bin/* /usr/local/bin

WORKDIR $GOPATH