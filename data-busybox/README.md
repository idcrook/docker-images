
# Data container based on busybox

Data container for `/var/lib/mysql`,  `/var/www/html`


## Building


```shell
docker build -t data-test .
```

## Example

``` shell
docker run -d --name data-one dpcrook/data-busybox tail -f /dev/null
docker run --name mysql-one --volumes-from data-one -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql
docker run --name wordpress-one --volumes-from data-one --link mysql-one:mysql -d -p 80 dpcrook/idcrook-wordpress

docker run --name wordpress-one --volumes-from data-one --link mysql-one:mysql -d -p 32780 dpcrook/idcrook-wordpress
```
