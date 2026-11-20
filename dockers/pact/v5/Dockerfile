FROM ubuntu:22.04@sha256:f64b2ffc2f10ab8f8505aa56b2664bd846cbb9df87c7e2532e30f7667021c0e6

RUN apt-get update && \
    apt-get clean

RUN mkdir /pact /exe

WORKDIR /tmp

ADD https://github.com/kadena-io/pact-5/releases/download/development-latest/pact-binary-bundle.ubuntu-latest.tar.gz /tmp/pact-binary-bundle.ubuntu-latest.tar.gz

RUN tar -xvf pact-binary-bundle.ubuntu-latest.tar.gz \
    && rm pact-binary-bundle.ubuntu-latest.tar.gz

RUN BIN_PATH=$(find /tmp -name bin) \
    && EXE_PATH=$(find /tmp -name exe) \
    && LIB_PATH=$(find /tmp -name lib) \
    && chmod +x "$BIN_PATH/pact" \
    && mv "$BIN_PATH/pact" /pact \
    && mv "$LIB_PATH"/* /lib/ \
    && mv "$EXE_PATH"/* /exe/ \
    && rm -rf /tmp/*

RUN ln -s /pact/pact /bin/pact

WORKDIR /data

CMD ["tail", "-f", "/dev/null"]