# docker-java-11-zulu

docker-java-11-zulu

see: https://hub.docker.com/u/azul

Installed at `/usr/lib/jvm/zulu-11`


Dockerfile [ci-and-cd/docker-java-11-zulu on Github](https://github.com/ci-and-cd/docker-java-11-zulu)

[cirepo/java-11-zulu on Docker Hub](https://hub.docker.com/r/cirepo/java-11-zulu/)


## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.9
COPY --from=cirepo/java-11-zulu:11.0.2-alpine-3.9-archive /data/root/usr/lib/jvm/zulu-11 /usr/lib/jvm/zulu-11

```
