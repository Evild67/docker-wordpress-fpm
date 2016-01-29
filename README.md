#How to use this image
```$ docker run --name some-wordpress --link some-mysql:mysql -d evild/docker-wordpress-fpm:latest```
The following environment variables are also honored for configuring your WordPress instance:

* ```-e WORDPRESS_DB_HOST=...``` (defaults to the IP and port of the linked mysql container)
* ```-e WORDPRESS_DB_USER=...``` (defaults to "root")
* ```-e WORDPRESS_DB_PASSWORD=...``` (defaults to the value of the MYSQL_ROOT_PASSWORD environment variable from the linked mysql container)
* ```-e WORDPRESS_DB_NAME=...``` (defaults to "wordpress")
* ```-e WORDPRESS_TABLE_PREFIX=...``` (defaults to "", only set this when you need to override the default table prefix in wp-config.php)
* ```-e WORDPRESS_AUTH_KEY=...```
* ```-e WORDPRESS_SECURE_AUTH_KEY=...```
* ```-e WORDPRESS_LOGGED_IN_KEY=...```
* ```-e WORDPRESS_NONCE_KEY=...```
* ```-e WORDPRESS_AUTH_SALT=...```
* ```-e WORDPRESS_LOGGED_IN_SALT=...```
* ```-e WORDPRESS_NONCE_SALT=...``` (default to unique random SHA1s)
If the WORDPRESS_DB_NAME specified does not already exist on the given MySQL server, it will be created automatically upon startup of the wordpress container, provided that the WORDPRESS_DB_USER specified has the necessary permissions to create it.

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:

```
$ docker run --name some-wordpress --link some-mysql:mysql -p 8080:80 -d evild/docker-wordpress-fpm:latest
```
Then, access it via http://localhost:8080 or http://host-ip:8080 in a browser.

If you'd like to use an external database instead of a linked mysql container, specify the hostname and port with WORDPRESS_DB_HOST along with the password in WORDPRESS_DB_PASSWORD and the username in WORDPRESS_DB_USER (if it is something other than root):

```
 $ docker run --name some-wordpress -e WORDPRESS_DB_HOST=10.1.2.3:3306 -e WORDPRESS_DB_USER=... -e WORDPRESS_DB_PASSWORD=... -d wordpress 
```

#... via docker-compose
Example docker-compose.yml for wordpress:
```
wordpress:
  image: evild/docker-wordpress-fpm:latest
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: example
```
Run docker-compose up, wait for it to initialize completely, and visit http://localhost:8080 or http://host-ip:8080.
