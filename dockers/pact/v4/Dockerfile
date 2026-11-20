FROM ubuntu:22.04@sha256:f64b2ffc2f10ab8f8505aa56b2664bd846cbb9df87c7e2532e30f7667021c0e6

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
