FROM ubuntu:16.04
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 6.11.0

RUN echo "nameserver 8.8.8.8" | tee /etc/resolv.conf > /dev/null
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get update && apt-get install -y --no-install-recommends zlibc \
    libssl1.0.0 libbz2-dev libxslt1-dev libxml2-dev python-gevent \
    libcurl4-openssl-dev python-psycopg2 python-dev python-setuptools \
    python-pip openssl build-essential curl software-properties-common python \
    g++ make

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN /bin/bash -c "source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default"

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN build_deps="curl ca-certificates" && \
    apt-get update && \
    apt-get install -y --no-install-recommends ${build_deps} && \
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install -y --no-install-recommends git-lfs && \
    git lfs install
# && \
#    apt-get purge -y --auto-remove ${build_deps} && \
#    rm -r /var/lib/apt/lists/*
RUN npm install -g process-nextick-args util-deprecate isarray core-util-is \
    inherits delegates console-control-strings string-width
RUN npm install -g yarn@1.1.0
RUN yarn global add npm@5.4.2
RUN npm -v && yarn -v

RUN pip install --upgrade pip
RUN easy_install distribute
RUN pip install --upgrade distribute

RUN mkdir /var/www

#ADD app.js /var/www/app.js

#CMD ["/usr/bin/node", "/var/www/app.js"]
