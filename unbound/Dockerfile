# Use the exact same pattern as successful HA addons
ARG BUILD_FROM
FROM $BUILD_FROM

# Install only what we need
RUN \
    apk update \
    && apk add --no-cache \
        unbound \
        bind-tools \
    && rm -rf /tmp/* /var/cache/apk/*

# Create necessary directories and download root hints
RUN mkdir -p /var/run/unbound /etc/unbound /var/lib/unbound /data && \
    curl -o /etc/unbound/root.hints https://www.internic.net/domain/named.root && \
    unbound-anchor -a /var/lib/unbound/root.key || true && \
    chown -R unbound:unbound /var/lib/unbound /var/run/unbound
    
# Copy rootfs  
COPY rootfs /

# Set permissions
RUN chmod a+x /etc/services.d/unbound/run

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION  
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version="${BUILD_VERSION}" \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
