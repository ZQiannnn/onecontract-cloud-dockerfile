FROM onecontract/adoptopenjdk:jdk8u292-b10

ENV TZ="Asia/Shanghai" \
    TINI_VERSION="v0.19.0" \
    JAVA_OPTS="-XX:MaxRAMPercentage=80.0"

# Install base packages
RUN apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        dnsutils \
        gnupg2 \
        netcat \
        net-tools \
        openssh-client \
        procps \
        tar \
        telnet \
        unzip \
        vim \
        wget; \
    apt-get upgrade -y --no-install-recommends; \
    rm -rf /var/lib/apt/lists/*; \
    ARCH="$(dpkg --print-architecture)"; \
    wget -qO "tini-static-${ARCH}" \
       "https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static-${ARCH}"; \
    wget -qO "tini-static-${ARCH}.sha256sum" \
       "https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static-${ARCH}.sha256sum"; \
    sha256sum --strict --check <"tini-static-${ARCH}.sha256sum"; \
    rm -r "tini-static-${ARCH}.sha256sum"; \
    mv "tini-static-${ARCH}" /usr/bin/tini; \
    chmod +x /usr/bin/tini; \
    tini --version

# Add mirror source
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak; \
    sed -i 's http://.*.debian.org http://mirrors.aliyun.com g' /etc/apt/sources.list

ENTRYPOINT ["tini", "--"]
