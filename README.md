# docker-mariadb

This container is primarily used for Database backend of OpenEMAIL. It is based on [tiredofit/mariadb](https://github.com/tiredofit/docker-mariadb). 
# docker-compose 

Add the following codes to your **OpenEMAIL** `docker-compose.yml`
```
mysql-openemail:
  image: openemail/mariadb:latest
  container_name: mysql
  volumes:
    - mysql-vol-1:/var/lib/mysql/
    - mysql-socket-vol-1:/var/run/mysqld/
    - ./data/conf/mysql/:/etc/mysql/conf.d/:ro
  environment:
    - TZ=${TZ}
    - MARIADB_ROOT_USER=root
    - MARIADB_ROOT_PASSWORD=${DBROOT}
  restart: always
  dns:
    - ${IPV4_NETWORK:-172.22.1}.254
  ports:
    - "${SQL_PORT:-127.0.0.1:13306}:3306"
  networks:
    openemail-network:
      aliases:
        - mysql
```
The above `${DBUSER}:${DBPASS}` represents **OpenEMAIL** database user and user's password as per `openemail.conf`
