#
# Copyright (c) 2021 Matthew Penner
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.
#

FROM        --platform=$TARGETOS/$TARGETARCH debian:latest

#LABEL       author="Matthew Penner" maintainer="matthew@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/C0d3-m4513r/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN 				apt-get update -y \
						&& apt-get install -y btrfs-progs lsof curl ca-certificates openssl git tar sqlite3 fontconfig libfreetype6 tzdata iproute2 libstdc++6 && rm -rf /var/lib/apt/lists/* \
						&& curl -o /graalvm.tar.gz -L https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-21.2.0/graalvm-ce-java8-linux-amd64-21.2.0.tar.gz \
						&& echo "0ea16ff2fe178800195e2918ee7ceee352601d4161a17eac67979febaaecfe05 graalvm.tar.gz" | sha256sum -c - \
						&& mkdir /graalvm && tar -xzf /graalvm.tar.gz -C /graalvm --strip-components=1 \
						&& rm /graalvm.tar.gz \
						&& chmod -R 755 /graalvm \
						&& useradd -d /home/container -m container \
						&& chown -R container /graalvm && chmod +x /graalvm/bin/java

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container
ENV         JAVA_HOME="/graalvm"
ENV         PATH="$JAVA_HOME/bin:$PATH"

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
