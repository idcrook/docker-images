# Wordpress

## Example

``` shell
docker build -t mod-wp .
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql
docker run --name some-wordpress --link some-mysql:mysql -d -p 80 mod-wp
```

## Details

 - php7.1
 - apache2
 - wordpress plugins:
   - WP Super Cache
   - WP Mail SMTP

customizations from wordpress official [php 7.1 apache](https://github.com/docker-library/wordpress/tree/master/php7.1/apache) docker


``` diff
diff --git a/php7.1/apache/Dockerfile b/php7.1/apache/Dockerfile
index 32089e5..94fb040 100644
--- a/php7.1/apache/Dockerfile
+++ b/php7.1/apache/Dockerfile
@@ -7,6 +7,8 @@ RUN set -ex; \
 	apt-get install -y \
 		libjpeg-dev \
 		libpng12-dev \
+		unzip \
+		rsync \
 	; \
 	rm -rf /var/lib/apt/lists/*; \
 	\
@@ -25,7 +27,12 @@ RUN { \
 		echo 'opcache.enable_cli=1'; \
 	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

-RUN a2enmod rewrite expires
+RUN { \
+		echo 'upload_max_filesize=32M'; \
+		echo 'post_max_size=32M'; \
+	} > /usr/local/etc/php/conf.d/docker-paas-book.ini
+
+RUN a2enmod rewrite expires headers

 VOLUME /var/www/html

diff --git a/php7.1/apache/docker-entrypoint.sh b/php7.1/apache/docker-entrypoint.sh
index 250c675..fb08205 100755
--- a/php7.1/apache/docker-entrypoint.sh
+++ b/php7.1/apache/docker-entrypoint.sh
@@ -1,6 +1,13 @@
 #!/bin/bash
 set -euo pipefail

+dl_and_move_plugin() {
+    name="$1"
+    curl -O $(curl -i -s "https://wordpress.org/plugins/$name/" | egrep -o "https://downloads.wordpress.org/plugin/[^']+")
+    unzip -o "$name".*.zip -d $(pwd)/wp-content/plugins
+}
+
+
 # usage: file_env VAR [DEFAULT]
 #    ie: file_env 'XYZ_DB_PASSWORD' 'example'
 # (will allow for "$XYZ_DB_PASSWORD_FILE" to fill in the value of
@@ -83,6 +90,10 @@ if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROT
 }

 EOPHP
+
+		dl_and_move_plugin "wp-super-cache"
+		dl_and_move_plugin "wp-mail-smtp"
+
 		chown www-data:www-data wp-config.php
```
