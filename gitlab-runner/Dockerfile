FROM gitlab/gitlab-runner:v1.11.4
MAINTAINER Eloy Adonis Colell <eloy.colell@gmail.com>

# git-lfs
RUN \
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install -y git-lfs
