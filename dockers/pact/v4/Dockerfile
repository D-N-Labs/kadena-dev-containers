FROM ubuntu:22.04@sha256:b000e69a50ff940390233025e7a337fb8469b509e2334fa4d018ac30f3e18cdb

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
