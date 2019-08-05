# use node 8 based on debian jessie as base image
FROM node:8-buster-slim

# software version
ENV LEARNINGLOCKER_VERSION v3.19.1
ENV XAPI_VERSION v2.5.11

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

# install xapi
RUN cd / \
 && git clone https://github.com/LearningLocker/xapi-service.git \
 && cd /xapi-service \
 && git checkout "${XAPI_VERSION}" \
 && yarn install --ignore-engines \
 && yarn build

WORKDIR /xapi-service

# set the entrypoint
ENTRYPOINT ["/usr/local/bin/node"]

CMD ["dist/server.js"]

# expose a default port
EXPOSE 8081
