FROM jenkins/jenkins:lts-alpine

MAINTAINER Jared De La Cruz <jared@jareddlc.com>

ARG USER_JENKINS=jenkins
ARG USER_ROOT=root
ARG GROUP_DOCKER=docker
ARG GROUP_DOCKER_GID=1001

# Switch to root
USER ${USER_ROOT}

# Create docker group and add user jenkins to the group
RUN addgroup -g ${GROUP_DOCKER_GID} ${GROUP_DOCKER} \
	&& addgroup ${USER_JENKINS} ${GROUP_DOCKER}

# Install docker
# From https://github.com/docker-library/docker/blob/4dd3f3fe1f263ea74cdc8bed6091008cf62ab751/19.03/Dockerfile

ENV DOCKER_CHANNEL stable
ENV DOCKER_BUCKET download.docker.com
ENV DOCKER_VERSION 19.03.4
ENV DOCKER_ARCH x86_64
ENV DOCKER_SHA256 efef2ad32d262674501e712351be0df9dd31d6034b175d0020c8f5d5c9c3fd10

RUN set -ex; \
	apk add --no-cache sudo; \
	apk add --no-cache --virtual .fetch-deps \
		curl \
		tar \
	&& curl -fSL -o docker.tgz "https://${DOCKER_BUCKET}/linux/static/${DOCKER_CHANNEL}/${DOCKER_ARCH}/docker-${DOCKER_VERSION}.tgz" \
	&& sha256sum docker.tgz \
	&& echo "${DOCKER_SHA256} *docker.tgz" | sha256sum -c - \
	&& tar --extract --file docker.tgz --strip-components 1 --directory /usr/local/bin/ \
	&& rm docker.tgz \
	&& apk del .fetch-deps \
	&& dockerd -v \
	&& docker -v

# Add user jenkins to list of sudoers
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Switch to user jenkins
USER ${USER_JENKINS}

COPY docker-socket.sh /
RUN curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

WORKDIR /var/jenkins_home
ENTRYPOINT ["/docker-socket.sh"]
