#
# ********  DOES NOT WORK ***
# /home/www-data/.local/lib/code-server-3.9.3/lib/node: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory
# =========================================================================

FROM alpine:3.13

# This env variable prevents intallation prompts
# ENV DEBIAN_FRONTEND=noninteractive
ENV CODESERVER_VERSION=3.9.3
# ENV TZ=America/Toronto
# PATH has to be modified like this
ENV PATH=/home/www-data/.local/bin":${PATH}"

RUN apk add --no-cache \
		ca-certificates \
		curl \
		tar \
		xz \
# https://github.com/docker-library/php/issues/494
		openssl

RUN set -eux; \
	addgroup -g 82 -S www-data; \
	adduser -u 82 -D -S -G www-data www-data
# 82 is the standard uid/gid for "www-data" in Alpine

# Attempt to maybe install the right c library
RUN apk add --no-cache \
    libstdc++

# https://github.com/sgerrand/alpine-pkg-glibc
RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-2.33-r0.apk && \
    apk add glibc-2.33-r0.apk

# https://github.com/cdr/code-server/blob/main/docs/install.md#standalone-releases
RUN echo "Install code-server" && \
    export WWWHOME=/home/www-data && \
    mkdir -p $WWWHOME/.local/lib $WWWHOME/.local/bin && \
    curl -fL https://github.com/cdr/code-server/releases/download/v$CODESERVER_VERSION/code-server-$CODESERVER_VERSION-linux-amd64.tar.gz \
    | tar -C $WWWHOME/.local/lib -xz  && \
    mv $WWWHOME/.local/lib/code-server-$CODESERVER_VERSION-linux-amd64 $WWWHOME/.local/lib/code-server-$CODESERVER_VERSION  && \
    ln -s $WWWHOME/.local/lib/code-server-$CODESERVER_VERSION/bin/code-server $WWWHOME/.local/bin/code-server

RUN chown -R www-data:www-data /home/www-data

EXPOSE 8080/tcp

CMD code-server
# Now visit http://127.0.0.1:8080. Your password is in ~/.config/code-server/config.yaml