# Uso de contenedores

## Ejecutar la imagen de mysql con comando bash:

```
sudo docker run --name mysql-test -it mysql bash
```
## Ver la lista de contenendores

```
sudo docker container ls
```

📗 **Note**. Ver los contenedores incluyendo los contenedores con extado finalizado.

```
sudo docker container ls -a
```

## Eliminar un contenendor  

En el ejemplo, se pretende eliminar el contenedor con PID a7b6d145cac2.  

```
sudo docker container rm a7b6d145cac2
```

## Ejecutar de manera interactiva un contenedor de alpine 

```
sudo docker run --rm -it  alpine
```

Una vez ejecutado el contenendor, se pide que consulte la información del SO. Para ello, ejecute el comando `cat /etc/os-release` 

Verá un resultado como el siguiente:  

/ # cat /etc/os-release
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.22.1
PRETTY_NAME="Alpine Linux v3.22"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
/ #

## Listar todas las imágenes  

```
sudo docker images
```

## Listar solo los IDs de las imágenes

```
sudo docker images -q
```


sudo docker container rm mysql-test

docker container run -it ubuntu bash
docker container run --rm --interactive --tty ubuntu bash
                             -i           -t

docker container run --rm --interactive --tty php php -a
											php: imagen
											php -a comando para modo interactivo.
											
docker container run --rm -it node node

Comandos:
console.log("Hola desde Node")
const os =require('node:os')
console.log(os.cpus())
console.log(os.homedir())
console.log(os.machine())
console.log(os.networkInterfaces())
.exit para salir


docker container run --name hello hello-world

Se elimina automáticamente
docker container run --name hello2 --rm hello-world

https://www.ionos.com/es-us/digitalguide/servidores/know-how/comandos-de-docker/


Publicar un servicio en un puerto:
sudo docker container run --rm --publish 8080:80 nginx

Ejecutar mysql en primer plano:
docker container run --name mysql8 --rm --env MYSQL_ROOT_PASSWORD=12345 --publish 3306:3306 mysql:8.0

Conectar desde Heidi SQL:
Usando el puerto 3306 y la clave 12345
Control C aparentemente sale del contenedor pero queda en ejecución (aún disponible para su uso).

sudo docker container stop <nombre o pid>

En segundo plano:
docker container run -d --name mysql8 --rm --env MYSQL_ROOT_PASSWORD=12345 --publish 3306:3306 mysql:8.0

Instalar cliente de mysql:
sudo apt update
sudo apt install -y mysql-client

mysql --host=127.0.0.1 --port=3306 --user=root --password=12345

Ejecutar mongodb:
sudo docker run --name mongodb-container -d -p 27017:27017 mongo:latest

Ingresar a la shell de mongo:
sudo docker exec -it mongodb-container mongosh

Para ver las bases de datos:
show databases

para trabajar con una base de datos:
use mybasedatos

Ver contenedores en ejecución:  
sudo docker ps

Ejecutar postgres:
sudo docker run --name mipostgres -e POSTGRES_PASSWORD=12345 -d -p 5432:5432 postgres:15

Conectar a postgres:  
sudo docker exec -it mipostgres psql -U postgres

SELECT current_database();

Ver las bases de datos:
\list

Conectarse a una base de datos: 
\connect postgres
SELECT * FROM pg_catalog.pg_database;

REFERENCIA PARA COMANDOS CLIENTE:
https://docs.docker.com/reference/cli/docker/
