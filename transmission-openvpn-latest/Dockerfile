ARG BUILD_FROM
ARG BUILD_VERSION
ARG BUILD_UPSTREAM="latest"
FROM ${BUILD_FROM}${BUILD_UPSTREAM}

RUN curl -J -L -o /tmp/bashio.tar.gz \
    "https://github.com/hassio-addons/bashio/archive/v0.7.1.tar.gz" \
    && mkdir /tmp/bashio \
    && tar zxvf \
    /tmp/bashio.tar.gz \
    --strip 1 -C /tmp/bashio \
    \
    && mv /tmp/bashio/lib /usr/lib/bashio \
    && ln -s /usr/lib/bashio/bashio /usr/bin/bashio \
    && rm -fr /tmp/bashio.tar.gz

RUN usermod -d /config-internal abc \
    && sed -i "s|/config|/config-internal|g" /etc/openvpn/adjustConfigs.sh \
    && sed -i "s|/config|/config-internal|g" /etc/openvpn/updateConfigs.sh \
    && sed -i "s|/config|/config-internal|g" /etc/openvpn/modify-openvpn-config.sh \
    && sed -i "s|/config|/config-internal|g" /etc/openvpn/start.sh \
    && sed -i "s|/config-internalure-openvpn.sh|/configure-openvpn.sh|g" /etc/openvpn/start.sh \
    && find /etc/openvpn/*/ -type f -name '*.sh' -exec sed -i "s|/config/|/config-internal/|g" {} \; \
    && find /etc/openvpn/*/ -type f -name '*.sh' -exec sed -i 's|/config\\/|/config-internal\\/|g' {} \; \
    && find /etc/openvpn/*/ -type f -name '*.ovpn' -exec sed -i "s|/config/|/config-internal/|g" {} \; \
    && sed -i "s|/config|/config-internal|g" /etc/transmission/userSetup.sh

# copy local files
COPY root/ /
RUN chmod +x /customstart.sh
CMD ["dumb-init", "/customstart.sh"]