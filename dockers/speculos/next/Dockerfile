FROM ghcr.io/ledgerhq/ledger-app-builder/ledger-app-builder:5.5.1 AS builder

SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

ARG APP_REPO=https://github.com/SmartPacts/app-kadena.git
ARG APP_VERSION=v2.0.0
ARG DEVICE=nanosp

WORKDIR /build

RUN git config --global --add safe.directory "*" && \
    git clone --depth 1 --branch "${APP_VERSION}" --recurse-submodules "${APP_REPO}" /build/app

WORKDIR /build/app

RUN mkdir -p /out && \
    case "${DEVICE}" in \
        nanosp|nanosplus|nanos2) cargo_target="nanosplus" ;; \
        nanox|stax|flex|apex_p)  cargo_target="${DEVICE}" ;; \
        *) echo "Unsupported DEVICE: ${DEVICE}" >&2; exit 1 ;; \
    esac && \
    cargo ledger build "${cargo_target}" && \
    cp "target/${cargo_target}/release/kadena" /out/app.elf && \
    test -s /out/app.elf

FROM ghcr.io/ledgerhq/speculos:v0.27.1

COPY --from=builder /out/app.elf /speculos/apps/app.elf

EXPOSE 5000
EXPOSE 9999

CMD ["--display", "headless", "--api-port", "5000", "--apdu-port", "9999", "/speculos/apps/app.elf"]
