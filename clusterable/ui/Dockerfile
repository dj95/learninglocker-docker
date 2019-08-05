# use node 8 based on debian jessie as base image
FROM node:8-buster-slim

# software version
ENV LEARNINGLOCKER_VERSION v3.19.1

# disable the interactive frontend for apt
ENV DEBIAN_FRONTEND noninteractive

# install dependencies
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        apt-transport-https \
        build-essential \
        curl \
        git \
        python \
        xvfb \
 && rm -rf /var/lib/apt/lists/*

# install the process manager
RUN yarn global add pm2

# install learninglocker
RUN cd / \
 && git clone https://github.com/LearningLocker/learninglocker.git \
 && cd /learninglocker \
 && git checkout "${LEARNINGLOCKER_VERSION}" \
 && yarn install \
 && yarn build-ui-server \
 && yarn build-ui-client \
 && yarn build-cli-server

# place the config file
COPY learninglocker.env /learninglocker/.env

WORKDIR /learninglocker

# set the entrypoint
ENTRYPOINT ["/usr/local/bin/node"]

CMD ["ui/dist/server"]

# expose a default port
EXPOSE 3000
