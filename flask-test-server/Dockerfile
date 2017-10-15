FROM python:2.7.14
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 6.11.0
ENV STD_PACKAGES zlibc \
    libssl1.0.0 libbz2-dev libxslt1-dev libxml2-dev python-gevent \
    libcurl4-openssl-dev openssl build-essential curl \
    software-properties-common g++ make
ENV BIN_PACKAGES python-pip python-psycopg2 python-dev python-setuptools

RUN echo "nameserver 8.8.8.8" | tee /etc/resolv.conf > /dev/null

# install apt
RUN apt-get update && apt-get install -y apt-utils
RUN apt-get update && \
    apt-get install -y --no-install-recommends $STD_PACKAGES && \
    apt-get install -y --no-install-recommends $BIN_PACKAGES && \
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

RUN pip install --upgrade pip && \
    easy_install distribute && \
    pip install --upgrade distribute

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN npm install -g process-nextick-args util-deprecate isarray core-util-is \
    inherits delegates console-control-strings string-width
RUN npm install -g yarn@1.1.0
RUN yarn global add npm@5.4.2

RUN apt-get purge -y --auto-remove ${build_deps}
# && \
#    apt-get purge -y --auto-remove $STD_PACKAGES && \
#    apt-get purge -y --auto-remove apt-utils && rm -r /var/lib/apt/lists/*

RUN npm -v && yarn -v

RUN mkdir /var/stage
EXPOSE 8000
VOLUME ["/var/stage"]
