#
# Stage 0 - build dist files
#

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
 && yarn build-ui-client

#
# Stage 1 - web server
#

# use the nginx image as base image for the second stage
# this stage is established in order to shrink the image size.
FROM nginx:alpine

RUN apk update \
 && apk add \
        bash \
 && rm -rf /var/cache/apk/*

# copy the nginx config to the correct location
COPY nginx.conf /etc/nginx/conf.d/mysite.template

# create the source directory for the learning locker ui
RUN mkdir -p /learninglocker/ui/dist

# copy the public files for the web application
COPY --from=0 /learninglocker/ui/dist/public /learninglocker/ui/dist/public

COPY docker-entrypoint /docker-entrypoint

RUN chmod +x /docker-entrypoint

ENTRYPOINT ["/docker-entrypoint"]
