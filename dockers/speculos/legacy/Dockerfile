FROM ghcr.io/ledgerhq/ledger-app-builder/ledger-app-builder:5.5.1 AS builder

SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

ARG APP_REPO=https://github.com/SmartPacts/app-kadena.git
ARG APP_VERSION=v1.3.4
ARG DEVICE=nanosp

WORKDIR /build

RUN git config --global --add safe.directory "*" && \
    git clone --depth 1 --branch "${APP_VERSION}" --recurse-submodules "${APP_REPO}" /build/app

WORKDIR /build/app

RUN mkdir -p /out && \
    case "${DEVICE}" in \
        nanosp|nanosplus|nanos2) sdk_var="NANOSP_SDK"; build_dir="nanos2" ;; \
        nanox)                   sdk_var="NANOX_SDK";  build_dir="nanox" ;; \
        stax)                    sdk_var="STAX_SDK";   build_dir="stax" ;; \
        flex)                    sdk_var="FLEX_SDK";   build_dir="flex" ;; \
        apex_p)                  sdk_var="APEX_P_SDK"; build_dir="apex_p" ;; \
        *) echo "Unsupported DEVICE: ${DEVICE}" >&2; exit 1 ;; \
    esac && \
    eval "BOLOS_SDK=\$${sdk_var}" && \
    make -C app -j2 BOLOS_SDK="${BOLOS_SDK}" PRODUCTION_BUILD=1 && \
    cp "app/build/${build_dir}/bin/app.elf" /out/app.elf && \
    test -s /out/app.elf

FROM ghcr.io/ledgerhq/speculos:v0.27.1

COPY --from=builder /out/app.elf /speculos/apps/app.elf

EXPOSE 5000
EXPOSE 9999

CMD ["--display", "headless", "--api-port", "5000", "--apdu-port", "9999", "/speculos/apps/app.elf"]
