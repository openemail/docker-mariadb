# docker-mariadb

This container is primarily used for Database backend of OpenEMAIL. It is based on [osixia/docker-mariadb](https://github.com/osixia/docker-mariadb). Learn more [here](https://github.com/osixia/docker-mariadb/blob/stable/README.md)

# docker-compose 

Add the following codes to your *OpenEMAIL* 'docker-compose.yml`
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
        - MARIADB_ROOT_USER=${DBROOT_USER}
        - MARIADB_ROOT_PASSWORD=${DBROOT_PASSWORD}
        - MARIADB_ROOT_ALLOWED_NETWORKS: 
           - localhost
           - 127.0.0.1
           - ::1
           - ${IPV4_NETWORK:-172.22.1}.0/24
           - ${IPV6_NETWORK:-fd4d:6169:6c63:6f77::/64}
        - MARIADB_DATABASES:
           - openemail:${DBPASS}
           - nextcloud:${DBPASS}
           - rainloop:${DBPASS}
           - roundcube:${DBPASS}
      restart: always
      dns:
        - ${IPV4_NETWORK:-172.22.1}.254
      ports:
        - "${SQL_PORT:-127.0.0.1:13306}:3306"
      networks:
        openemail-network:
          aliases:
            - mysql
