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
        gettext-base \
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
 && yarn build-all

# place the config file
COPY learninglocker.env /learninglocker/.env

# install xapi
RUN cd / \
 && git clone https://github.com/LearningLocker/xapi-service.git \
 && cd /xapi-service \
 && git checkout "${XAPI_VERSION}" \
 && yarn install --ignore-engines \
 && yarn build

# place the config file
COPY xapi.env /xapi-service/.env

# install proxy
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        nginx \
 && rm -rf /var/lib/apt/lists/*

# place the config file
COPY nginx.conf /etc/nginx/conf.d/mysite.template

# copy the entrypoint
COPY docker-entrypoint /docker-entrypoint

# make the entrypoint executable
RUN chmod +x /docker-entrypoint

WORKDIR /learninglocker

# set the entrypoint
ENTRYPOINT ["/docker-entrypoint"]

# expose a default port
EXPOSE 80
