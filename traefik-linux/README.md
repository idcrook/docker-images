# traefik-linux

Build an ARM docker containers for Traefik based on Alpine

Inspired by https://github.com/hypriot/rpi-traefik

## Run it

Examples

```
docker run -d -p 8080:8080 -p 80:80 \
    -v $PWD/traefik.toml:/etc/traefik/traefik.toml \
	dpcrook/traefik-linux:armhf

docker run -d -p 8080:8080 -p 80:80 \
    -v $PWD/traefik.toml:/etc/traefik/traefik.toml \
	dpcrook/traefik-linux:`dpkg --print-architecture`
```


## Docker Hub

Hub: [dpcrook/traefik-linux](https://hub.docker.com/r/dpcrook/traefik-linux/)

 - `docker pull dpcrook/traefik-linux`


### Build it

``` shell
git clone https://github.com/idcrook/docker-images.git
cd docker-images/traefik-linux
dpkg --print-architecture
```

On Raspbian (ARMV7l 32-bit), Raspberry Pi 3 with Docker installed:

``` shell
docker build -t armhf -f Dockerfile.armhf .
```

On Raspberry Pi Models 2 and 3 running Raspbian, `uname -m` is `armv7l`

On Raspberry Pi Models 1 B running Raspbian, `uname -m` is `armv6l`

`armhf` has `hf` which stands for "Hardware Floating-point", a legacy
thing arising from the earliest system architectures on the Pi were released without the FP hardware enabled.



On Pine 64 Ubuntu (ARMV8 64-bit) with Docker installed:

``` shell
docker build -t arm64 -f Dockerfile.arm64 .
```

On Pine A64, `uname -m` is `aarch64`

#### publish to docker hub

``` shell
docker login -u dpcrook
```

``` shell
docker tag armhf dpcrook/traefik-linux:armhf
docker push dpcrook/traefik-linux:armhf
```

``` shell
docker tag arm64 dpcrook/traefik-linux:arm64
docker push dpcrook/traefik-linux:arm64
```
