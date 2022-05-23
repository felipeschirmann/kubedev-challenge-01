## kubedev-challenge-01.1

This challenge consistis in create containers of databases more used.

First create the network: docker network create web_default

### MongoDB: 

docker network create web_default && docker run --name container-mongo --network web_default --mount source=mongodb,target=/data/db -e  MONGO_INITDB_ROOT_USERNAME=root  -e MONGO_INITDB_ROOT_PASSWORD=my-secret-mongo  -d mongo

### MariaDB:

docker run --name container-mariadb --network web_default --mount source=mariadb,target=/etc/mysql/conf.d -e MARIADB_ROOT_PASSWORD=my-secret-mariadb -d mariadb

### PostgreSQL:

docker run --name container-postgres -e POSTGRES_PASSWORD=my-secret-postgres -e PGDATA=/var/lib/postgresql/data/pgdata --mount source=postgresdb,target=/var/lib/postgresql/data --network web_default -d postgres

### Redis:

docker run --name container-redis --mount source=redisdb,target=/data -e REDIS_PASSWORD=my-secret-redis --network=web_default -d redis redis-server --save 60 1 --loglevel warning 

## kubedev-challenge-01.2

This challenge consistis in create containers of management tools of databases.

### MongoDB ⇒ Mongo Express (https://github.com/mongo-express/mongo-express):

docker run --name mongo-express --network web_default -p 8081:8081 -e ME_CONFIG_MONGODB_URL="mongodb://root:my-secret-mongo@container-mongo:27017/?authSource=admin" mongo-express

### MariaDB ⇒ phpmyadmin (https://www.phpmyadmin.net):

docker run --name myadmin  --network web_default -p 8080:80 -e PMA_HOST="container-mariadb" -d phpmyadmin

### PostgreSQL ⇒ PgAdmin (https://www.pgadmin.org):

docker run --name container-pgadmin --network=web_default -p 15432:80 -e PGADMIN_DEFAULT_EMAIL=teste@teste.com -e PGADMIN_DEFAULT_PASSWORD=my-secret-pgadmin4 -d dpage/pgadmin4

### Redis ⇒ redis-commander (https://hub.docker.com/r/rediscommander/redis-commander):

docker run --name redis-commander -e REDIS_HOSTS=container-redis -e HTTP_PASSWORD=my-secret-redis --network=web_default -p 8081:8081 -d rediscommander/redis-commander

