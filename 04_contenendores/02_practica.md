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

```
/ # cat /etc/os-release
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.22.1
PRETTY_NAME="Alpine Linux v3.22"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
/ #
```

## Listar todas las imágenes  

```
sudo docker images
```

## Listar solo los IDs de las imágenes

```
sudo docker images -q
```

## Eliminar un contenedor

Se pide eliminar el contenedor mysql-test.  

```
sudo docker container rm mysql-test
```

## Ejecute un contenedor de ubuntu de manera interactiva. El comando a ejecutar es bash.

```
docker container run -it ubuntu bash
```

***es equivalente a***  
```
docker container run --rm --interactive --tty ubuntu bash
```

**Recordatorio** -i = interactive, -t = --tty`

## Ejecute un contenedor de PHP de manera interactiva

Se ejecutará el comando `php -a` que permite ejecutar comandos de PHP de manera interactiva mediante una terminal.  
```
docker container run --rm --interactive --tty php php -a
```
donde **php** es la imagen y **php -a** es el comando que se está ejecutando en el contenedor.  Ahora puede ejeucutar comandos de php dentro del contenedor.  

ejemplos:  
```
echo "Bienvenidos a PHP";
echo 4 + 10;
echo phpinfo();
```

## Ejecute un contenedro de node de manera interactiva  

```
docker container run --rm -it node node
```

donde la primera palabra **node** es el nombre de la imagen y la segunda palabra **node** es el comando que se está ejecutando dentro del contenedor.  

📗 Comandos que puede probar en el contenedor de node:  

console.log("Hola desde Node")
const os =require('node:os')
console.log(os.cpus())
console.log(os.homedir())
console.log(os.machine())
console.log(os.networkInterfaces())

**.exit** para salir

## Ejecutar un contenedor de hello-world

El parámetro **--rm** indica que elimine el contenedor una vez finalizada la ejecución del comando.  

```
docker container run --name hello --rm hello-world
```

# PUBLICAR UN SERVICIO EN UN PUERTO

## Publique el servicio de nginx

```
+-----------Host----------+
-                         -
-    +---container---+    -
-    -     nginx     -    -
-    -               -    -
-    -               -    -
-    +-------80------+    -
-             |           -
-             |           -
+-----------8080----------+
              |
              |
        Navegador web
```

```
sudo docker container run --rm --publish 8080:80 nginx
```

📗 Nota. Consulte en el servidor desde el navegador web del host (http://localhost:8080)  

## Ejecutar mysql en primer plano

```
+-----------Host------------+
-                           -
-                           -
-    +----container-----+   -
-    -      mysql       -   -
-    -                  -   -
-    +-------3306-------+   -
-             |             -
-             |             -
+------------3306-----------+
              |
              |
     Cliente de base de datos
```


```
docker container run --name mysql8 --rm --env MYSQL_ROOT_PASSWORD=12345 --publish 3306:3306 mysql:8.0
```

▶️ Se pide utilizar una interfaz gráfica para conectarse a la base de datos desde el equipo host. Puede utilizar HeidiSQL, DBEver, phpMyAdmin, etc.  La clave establecida para el usuario **root** es **12345**  

**CTRL+C** para cerrar el contenedor. Aún queda en ejecución.  

📙 Se puede cerrar el contenedor con el siguiente comando:  

```
sudo docker container stop <nombre contenedor>
```

```
sudo docker container stop <PID>
```

## Ejecutar mysql en segundo plano

```
+-----------Host------------+
-                           -
-                           -
-    +----container-----+   -
-    -      mysql       -   -
-    -                  -   -
-    +-------3306-------+   -
-             |             -
-             |             -
+------------3306-----------+
              |
              |
     Cliente de base de datos
```

```
docker container run -d --name mysql8 --rm --env MYSQL_ROOT_PASSWORD=12345 --publish 3306:3306 mysql:8.0
```

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
