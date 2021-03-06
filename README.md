# jenkins-with-docker-socket
Official Jenkins image with Docker socket
#### https://jordimarti.tech/continuous-integration-environment-with-docker-compose-using-traefik-and-jenkins/

[![Docker Build](https://img.shields.io/docker/build/jareddlc/jenkins-with-docker-socket.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)
[![Docker Stars](https://img.shields.io/docker/stars/jareddlc/jenkins-with-docker-socket.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)
[![Docker Pulls](https://img.shields.io/docker/pulls/jareddlc/jenkins-with-docker-socket.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)
[![Docker Layers](https://shields.beevelop.com/docker/image/layers/jareddlc/jenkins-with-docker-socket/latest.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)
[![Docker Size](https://shields.beevelop.com/docker/image/image-size/jareddlc/jenkins-with-docker-socket/latest.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)
[![Docker Automated](https://img.shields.io/docker/automated/jareddlc/jenkins-with-docker-socket.svg)](https://hub.docker.com/r/jareddlc/jenkins-with-docker-socket/)


This image was created to be a small layer on top of jenkins:alpine such that it could have access to the host's docker cli (via docker.sock). This image will run using the jenkins user, however it introduces a docker group and sets ownership of the docker.sock to it, additionally it adds the jenkins user to the list of sudoers.

# Supported tags and respective `Dockerfile` links

* `alpine` [(jareddlc/jenkins-with-docker-socket/alpine/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/alpine/Dockerfile)
* `latest` [(jareddlc/jenkins-with-docker-socket/latest/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/latest/Dockerfile)
* `lts` [(jareddlc/jenkins-with-docker-socket/lts/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/lts/Dockerfile)
* `lts-alpine` [((jareddlc/jenkins-with-docker-socket/lts-alpine/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/lts-alpine/Dockerfile)
* `lts-slim` [(jareddlc/jenkins-with-docker-socket/lts-slim/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/lts-slim/Dockerfile)
* `slim` [(jareddlc/jenkins-with-docker-socket/slim/Dockerfile)](https://github.com/jareddlc/jenkins-with-docker-socket/blob/master/slim/Dockerfile)

# How to use this image

`$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 8080:8080 -p 50000:50000 jareddlc/jenkins-with-docker-socket:lts-alpine`

# Alternate instructions:
``` 
cd lts-alpine
./docker-build.sh
cd ..
docker stack deploy -c docker-compose.yml jenkins

```
### Get password by docker logs container-id

### sample docker command, using FreeStyle Option for pipeline, run shell command:
```
docker service create --network docker-browser2_default \
--name whoami2 --container-label traefik.http.routers.whoami2.rule='Host(`whoami2.localhost`)' \
--container-label traefik.http.routers.whoami2.service="whoami2" \
--container-label traefik.http.services.whoami2.loadbalancer.server.port="8000" \
--container-label traefik.docker.network="docker-browser2_default" \
--label traefik.http.routers.whoami2.rule='Host(`whoami2.localhost`)' \
--label traefik.http.routers.whoami2.service="whoami2" \
--label traefik.http.services.whoami2.loadbalancer.server.port="8000" \
--label traefik.docker.network="docker-browser2_default" jwilder/whoami
```

### or run docker-compose, using the following in Freestyle Option. any containers launched will show up on host
### copy docker-compose.yml file to $PWD/jenkins_home, which will copy into container since volume is mapped
```
docker-compose -f /var/jenkins_home/docker-compose2.yml up
```

For more information and options see official jenkins repo: https://hub.docker.com/_/jenkins/
