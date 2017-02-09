# mysql 5.6 docker image

Intended to be compatible with kubernetes using NFS volume mounts

copied from https://github.com/mysql/mysql-docker/tree/mysql-server/5.6

Docker Hub : mysql/mysql-server:5.6
- https://hub.docker.com/r/mysql/mysql-server/


## Build and publish

To my docker hub

```
docker build -t mysql:5.6 .
docker tag mysql:5.6 dpcrook/mysql-k8s-nfs:5.6
docker push dpcrook/mysql-k8s-nfs:5.6
```

## Why?

In kubernetes was getting errors with the final `chmod` in the `docker-entrypoint.sh`

```
kubectl logs go-auth-mysql-2152798905-7xpz1
chown: changing ownership of '/var/lib/mysql/': Operation not permitted
```

So built with that commented out.
