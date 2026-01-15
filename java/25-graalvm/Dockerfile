FROM        --platform=$TARGETOS/$TARGETARCH debian:latest

#LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/C0d3-m4513r/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apt update -y \
            && apt install -y \
                btrfs-progs \
                libstdc++6 \
                curl \
                lsof \
                ca-certificates \
                openssl \
                git \
                tar \
                sqlite3 \
                fontconfig \
                tzdata \
                iproute2 \
                libfreetype6 \
                tini \
                zip \
                unzip \
                jq \
            && rm -rf /var/lib/apt/lists/* \
						&& curl -o /graalvm.tar.gz -O https://download.oracle.com/graalvm/25/latest/graalvm-jdk-25_linux-x64_bin.tar.gz \
						&& mkdir /graalvm && tar -xzf /graalvm.tar.gz -C /graalvm --strip-components=1 \
						&& rm /graalvm.tar.gz \
						&& chmod -R 755 /graalvm \
						&& useradd -m -d /home/container -s /bin/bash container \
						&& chown -R container:container /graalvm && chmod +x /graalvm/bin/java

ENV         JAVA_HOME="/graalvm"
ENV         PATH="$JAVA_HOME/bin:$PATH"

## Setup user and working directory
#RUN         useradd -m -d /home/container -s /bin/bash container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
