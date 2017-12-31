FROM gitlab/gitlab-runner:latest
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

# git-lfs
RUN \
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install -y git-lfs
