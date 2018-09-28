FROM debian:stable
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 8.11.1

ENV STD_PACKAGES zlibc \
    libssl1.1 libbz2-dev libxslt1-dev libxml2-dev python-gevent \
    libcurl4-openssl-dev openssl build-essential curl \
    software-properties-common g++ make xvfb chromium libpng* xauth

# install apt
RUN apt-get update && apt-get install -y apt-utils
RUN apt-get update && \
    apt-get install -y --no-install-recommends $STD_PACKAGES && \
    build_deps="curl ca-certificates" && \
    apt-get update && \
    apt-get install -y --no-install-recommends ${build_deps} && \
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install -y --no-install-recommends git-lfs && \
    git lfs install && \
    curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN /bin/bash -c "source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default"

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN npm install -g process-nextick-args util-deprecate isarray core-util-is \
    inherits delegates console-control-strings string-width
RUN npm install -g yarn@1.5.1
RUN yarn global add npm@5.6.0
RUN npm install v8-profiler@5.7.0

RUN npm -v && yarn -v