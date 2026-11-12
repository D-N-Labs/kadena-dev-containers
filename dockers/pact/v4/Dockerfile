FROM ubuntu:22.04@sha256:74b12b11ffa550fba1929ad4d8d16533c34f6612728469671afba1b03b66881c

RUN apt-get update && \
    apt-get install -y unzip && \
    apt-get clean

WORKDIR /tmp

ADD https://github.com/kadena-io/pact/releases/download/v4.13.1/pact-4.13.1-linux-22.04.zip pact.zip

RUN unzip pact.zip \
    && chmod +x pact \
    && mv pact /bin/pact \
    && rm -rf /tmp/*

WORKDIR /data

CMD ["tail", "-f", "/dev/null"]
