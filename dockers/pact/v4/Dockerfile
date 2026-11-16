FROM ubuntu:22.04@sha256:9abcaffc92a5279fff3a6a41635c0800ea33e528425fb389658806476e7defde

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
