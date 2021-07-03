ARG BUILD_FROM=hassioaddons/base:7.0.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Sets working directory
WORKDIR /opt

# Setup base
# hadolint ignore=DL3003
RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        git=2.32.0 \
        lua-resty-http=0.16.1 \
        nginx-mod-http-lua=1.20.1-r4 \
        nginx=1.20.1 \
        nodejs=14.7.2 \
        npm=13.14.0 \
        paxctl=0.9-r0 \
    \
    && paxctl -cm "$(command -v node)" \
    && npm config set unsafe-perm true \
    \
    && npm install -g modclean@3.0.0-beta.1 \
    \
    && git clone --branch "v2.16.0" --depth=1 \
        https://github.com/MichMich/MagicMirror.git /opt \
    \
    && npm install \
      --no-optional \
      --only=production \
    \
    && git clone \
      https://github.com/jonochocki/MMM-CTA.git /opt/modules/MMM-CTA \
    \
    && cd /opt/modules/MMM-CTA \
    && npm install \
    \
    && modclean \
      --path /opt \
      --no-progress \
      --keep-empty \
      --run \
    \
    && npm uninstall -g modclean \
    && npm cache clear --force \
    \
    && rm -f -r \
      /tmp/* \
      /opt/.git \
      /opt/modules/MMM-CTA/.git \
      /root/.electron

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="MagicMirror²" \
    io.hass.description="MagicMirror² is an open source modular smart mirror platform." \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Jon Ochocki" \
    org.label-schema.description="MagicMirror² is an open source modular smart mirror platform." \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="MagicMirror²" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://community.home-assistant.io/t/community-hass-io-add-on-magicmirror/XXXX" \
    org.label-schema.usage="https://github.com/jonochocki/addon-magicmirror/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/jonochocki/addon-magicmirror" \
    org.label-schema.vendor="Jon Ochocki"
