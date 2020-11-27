FROM alpine
LABEL maintainer="kev <noreply@datageek.info>, Sah <contact@leesah.name>"

ARG TZ='Asia/Shanghai'

ENV SERVER_ADDR 18.162.149.207
ENV SERVER_PORT 443
ENV LOCAL_ADDRESS 0.0.0.0
ENV PASSWORD=
ENV METHOD      aes-256-gcm
ENV TIMEOUT     3000
# ENV DNS_ADDRS    8.8.8.8,8.8.4.4
ENV LOCAL_PORT 1986
ENV ARGS=
ENV TZ ${TZ}

ENV VER=3.3.5

# Build environment setup
RUN set -ex \
&& apk add --no-cache --virtual .build-deps \
    autoconf \
    automake \
    build-base \
    c-ares-dev \
    libev-dev \
    libtool \
    libsodium-dev \
    linux-headers \
    mbedtls-dev \
    pcre-dev \
    git \
# Set timezone and user
&& ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
&& echo ${TZ} > /etc/timezone \
# Get source code 
&& git clone https://github.com/shadowsocks/shadowsocks-libev.git -b "v${VER}" --depth=1 --recursive /tmp/repo \
# Build & install
&&  cd /tmp/repo \
&& ./autogen.sh \
&& ./configure --prefix=/usr --disable-documentation \
&& make install \
&& apk del .build-deps \
# Runtime dependencies setup
&& apk add --no-cache \
  rng-tools \
  $(scanelf --needed --nobanner /usr/bin/ss-* \
  | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
  | sort -u) \
# Cleanup
&& rm -rf /tmp/repo \
		  /etc/service \
		  /var/cache/apk/* \
USER nobody

CMD  exec ss-local \
    -s $SERVER_ADDR \
    -p $SERVER_PORT \
    -b $LOCAL_ADDRESS \
    -l $LOCAL_PORT \
    -k ${PASSWORD:-$(hostname)} \
    -m $METHOD \
    -t $TIMEOUT \
    -u \
    $ARGS
