

### Primero abrimos una terminal y actualizamos el indice de paquetes
```
sudo apt-get update
```
### Despliegue de un contenedor de postgres y el pgadmin
```
 $ docker network create postgres

$ docker run -d \
    --name postgres \
    --restart always \
    --network postgres \
    -e POSTGRES_PASSWORD=postgres \
    -e PGDATA=/var/lib/postgresql/data/pgdata \
    -v /custom/mount:/var/lib/postgresql/data \
    -p 5432:5432 \
    postgres



$ docker run -p 4000:80 \
    --name pgadmin \
    --restart always \
    --network postgres \
    -e "PGADMIN_DEFAULT_EMAIL=postgres@postgres.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=postgres" \
    -d dpage/pgadmin4
    
-- Crear Backup de base de datos en postgres.
$ docker exec -t -u postgres pg-docker pg_dump -h localhost -p 5432 -U postgres -F c -b -v -f '/var/lib/postgresql/data/comprobantes_{{ansible_date_time.date}}.backup' comprobantes_test
    
-- Levantar Sql Server
$ docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=sa' -e 'MSSQL_PID=Express' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest-ubuntu
    
#cambiar clave postgres
$ psql
    ALTER USER postgres WITH PASSWORD 'postgres';
    CREATE ROLE produccion WITH LOGIN ENCRYPTED PASSWORD 'produccion';
--    sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
