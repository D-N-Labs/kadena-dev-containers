FROM ubuntu:22.04@sha256:076e7bf82319619f28dd0aa9c02ab34b055ad37100395b4f49a2ffba04fb5e11

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
