ARG APP_REPO=https://github.com/SmartPacts/app-kadena.git
ARG APP_VERSION=v2.0.0
ARG DEVICE=nanosp

FROM ghcr.io/ledgerhq/ledger-app-builder/ledger-app-builder:5.5.1 AS builder

SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

ARG APP_REPO
ARG APP_VERSION
ARG DEVICE

WORKDIR /build

RUN git config --global --add safe.directory "*" && \
    git clone --depth 1 --branch "${APP_VERSION}" --recurse-submodules "${APP_REPO}" /build/app

WORKDIR /build/app

RUN case "${DEVICE}" in \
      nanosp|nanoSP)   target=nanosplus ;; \
      nanox|nanoX)     target=nanox ;; \
      apexp|apex_p)    target=apex_p ;; \
      stax|flex)       target="${DEVICE}" ;; \
      *) echo "Unsupported DEVICE: '${DEVICE}' (use nanosp, nanox, stax, flex or apexp)" \
           >&2; exit 1 ;; \
    esac && \
    echo "${target}" > /tmp/cargo_target

RUN cargo_target="$(cat /tmp/cargo_target)" && \
    cargo ledger build "${cargo_target}" && \
    mkdir -p /out && \
    cp "target/${cargo_target}/release/kadena" /out/app.elf && \
    test -s /out/app.elf

FROM ghcr.io/ledgerhq/speculos:v0.27.1

ARG APP_REPO
ARG APP_VERSION
ARG DEVICE

LABEL org.opencontainers.image.title="Kadena community ledger app Speculos" \
      org.opencontainers.image.description="Speculos emulator running the Kadena Ledger app for ${DEVICE}" \
      org.opencontainers.image.url="${APP_REPO}" \
      org.opencontainers.image.version="${APP_VERSION}" \
      org.opencontainers.image.revision="${APP_VERSION}" \
      io.ledger.device="${DEVICE}"

COPY --from=builder /out/app.elf /speculos/apps/app.elf

EXPOSE 5000
EXPOSE 9999

CMD ["--display", "headless", "--api-port", "5000", "--apdu-port", "9999", "/speculos/apps/app.elf"]
