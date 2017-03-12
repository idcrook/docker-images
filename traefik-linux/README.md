# traefik-linux

Build an ARM docker containers for Traefik based on Alpine

Inspired by https://github.com/hypriot/rpi-traefik

## Run it

For eg:

```
docker run -d -p 8080:8080 -p 80:80 \
    -v $PWD/traefik.toml:/etc/traefik/traefik.toml \
	dpcrook/traefik-linux
```


## Docker Hub

Hub: [dpcrook/traefik-linux](https://hub.docker.com/r/dpcrook/traefik-linux/)

 - `docker pull dpcrook/traefik-linux`


### Build it

On Raspbian (ARM7 32-bit), Raspberry Pi 2 or 3:

``` shell
docker build -t armhf -f Dockerfile.armhf .
```

On Pine 64 Ubuntu (aarch64):

``` shell
docker build -t aarch64 -f Dockerfile.aarch64 .
```

#### publish to docker hub

``` shell
docker tag armhf dpcrook/traefik-linux:armhf
docker push dpcrook/traefik-linux:armhf
```

``` shell
docker tag armhf dpcrook/traefik-linux:aarch64
docker push dpcrook/traefik-linux:aarch64
```
