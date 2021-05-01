FROM ubuntu:latest

# This env variable prevents intallation prompts
ENV DEBIAN_FRONTEND=noninteractive
ENV CODESERVER_VERSION=3.9.3
# ENV TZ=America/Toronto
# Build process user and group ids
ENV BUID=1000
ENV BGID=1000
# PATH has to be modified like this
ENV PATH=/home/coder/.local/bin":${PATH}"


# https://blog.zackad.dev/en/2017/08/17/add-ppa-simple-way.html
RUN apt-get update && \
    apt-get install -y gnupg2 && \
    echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu focal main" >> /etc/apt/sources.list && \
    echo "deb-src http://ppa.launchpad.net/ondrej/php/ubuntu focal main" >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 14AA40EC0831756756D7F66C4F4EA0AAE5267A6C && \
    apt-get update && \
    apt-get install -y php7.4
    # printf '2\n\n149\n' | apt-get install -y php7.4

RUN echo "Add coder user" && \
    addgroup --system --gid $BGID coder && \
    adduser --system --uid=$BUID --gid $BGID --disabled-password coder

# https://github.com/cdr/code-server/blob/main/docs/install.md#standalone-releases
RUN echo "Install code-server" && \
    export CODERHOME=/home/coder && \
    mkdir -p $CODERHOME/.local/lib $CODERHOME/.local/bin && \
    php -r "echo file_get_contents('https://github.com/cdr/code-server/releases/download/v$CODESERVER_VERSION/code-server-$CODESERVER_VERSION-linux-amd64.tar.gz');" \
    | tar -C $CODERHOME/.local/lib -xz  && \
    mv $CODERHOME/.local/lib/code-server-$CODESERVER_VERSION-linux-amd64 $CODERHOME/.local/lib/code-server-$CODESERVER_VERSION  && \
    ln -s $CODERHOME/.local/lib/code-server-$CODESERVER_VERSION/bin/code-server $CODERHOME/.local/bin/code-server

RUN chown -R coder:coder /home/coder/.local

RUN echo "**** clean up ****" && \
    apt-get purge --auto-remove -y \
        build-essential \
        libx11-dev \
        libxkbfile-dev \
        libsecret-1-dev \
        software-properties-common \
        pkg-config && \
    apt-get clean && \
    rm -rf \
        /tmp/* \
        /var/lib/apt/lists/* \
        /var/tmp/*

EXPOSE 8080/tcp

#RUN export PATH=~/.local/bin":"$PATH
#RUN echo $PATH;
USER coder

# CMD code-server
CMD export PATH=~/.local/bin":"$PATH && code-server
# Now visit http://127.0.0.1:8080. Your password is in ~/.config/code-server/config.yaml