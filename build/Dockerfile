FROM alpine:3.2

ENV VERSION=2.2.1
ENV PREFIX=/release
VOLUME ${PREFIX}

ADD https://github.com/lalyos/docker-upx/releases/download/v3.91/upx /bin/upx
RUN chmod +x /bin/upx
RUN apk update && apk add make gcc g++ python linux-headers paxctl libgcc libstdc++ wget
RUN \
    wget --no-check-certificate https://iojs.org/dist/v${VERSION}/iojs-v${VERSION}.tar.xz -P /tmp && \
    tar -C /tmp -xJf /tmp/iojs-v${VERSION}.tar.xz && \
    cd /tmp/iojs-v${VERSION} && \
    ./configure --prefix=${PREFIX} --fully-static && \
    make -j$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    make install && \
    paxctl -cm ${PREFIX}/bin/iojs && \
    cd ${PREFIX} && \
    find ${PREFIX}/lib/node_modules/npm -name test -o -name .bin -type d | xargs rm -rf && \
    rm -rf ${PREFIX}/include ${PREFIX}/share/man ${PREFIX}/lib/node_modules/npm/man ${PREFIX}/lib/node_modules/npm/doc ${PREFIX}/lib/node_modules/npm/html && \
    /bin/upx --best --ultra-brute ${PREFIX}/bin/iojs
