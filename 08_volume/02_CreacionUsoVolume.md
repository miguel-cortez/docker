# Creación y uso de volúmenes

## 1. Crear un volumen

Se creará un volumen llamado 📦**myvolume**

```bash
docker volume create myvolume
```
```bash
docker volume create --name myvolume
```
***📘 Nota*** Los dos comandos anteriores tienen el mismo significado (son equivalentes).  

## 2. Crear un volumen en memoria RAM  

Se creará un volumen llamado 📦**myvolume2**.

```
docker volume create --driver local --opt type=tmpfs –opt device=tmpfs --opt o=size=100m,uid=1000 myvolume2
```

## 3. Listar los volúmenes
```bash
docker volume ls
```

## 4. Utilizar un volumen

### Ejecutar un contenedor de alpine que utilice el volumen 📦myvolume

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume alpine
```
Explicación del comando:  
🔰 **type**  
- `type=volume` para montar un volumen.
- `type=bind` para montar un directorio del equipo host en el contenedor
- `type=tmpfs` para montar un sistema de archivos temporal en memoria RAM (no es persistente).

🔰 **src** deriva de la palabra source u origen
- Es el nombre del volumen que se quiere montar en el contenedor.

🔰 **dst** deriva destino (en inglés)
- Indica cómo se llamará el directorio donde el volumen será montado dentro del contenedor.
- El directorio se creará automáticamente dentro del contenedor.
- El directorio de destino puede llamarse diferente del nombre del volumen.


### Ejecutar un contenedor de ubuntu que utilice el volumen 📦myvolume

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume ubuntu
```


```bash
docker run -it --rm -v myvolume:/myvolume ubuntu
```

***📘 Nota*** Los dos comandos anteriores tienen el mismo significado (son equivalentes).  


### Ejecuta un contenedor de ubuntu que utiliza 📦myvolume2

```
docker run -it --rm -v myvolume2:/data ubuntu bash
```
📚 Debido a que **myvolume2** es de tipo **tmpfs** los datos no son persistentes.  

### Ejecuta un contenedor de MySQL en segundo plano (sin utilizar volumen)

⚡Esto se hará para buscar el archivo **my.cnf** y la ruta de la carpeta **datadir** de **MySQL**

```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```

#### Ejecutar un comando en un contenedor que está corriendo en segundo plano

```
docker exec -it some-mysql /bin/bash
```

#### Buscando el archivo my.cnf
```
find / -name my.cnf
```
📖El archivo ***my.cnf** fue localizado en ***/etc/mysql/my.cnf***

#### Ver el contenido del archiov my.cnf

```
cat /etc/mysql/my.cnf
```

#### Salga del contenedor de MySQL que se está ejecutando en segundo plano (el único en ejecución)

```
exit
```

### Detenga el contenedor de MySQl que se está ejecutando en segundo plano

```
docker container rm some-mysql
```

### Elimine el contenedor de MySQL que detuvo recientemente (en paso anterior)

```
docker container rm some-mysql
```

## Ejecute nuevamente mysql; pero usando el volumen myvolume

```
docker run -v myvolume:/var/lib/mysql -p 3306:3306 --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```
o
```
docker run --mount type=volume,src=myvolume,dst=/var/lib/mysql --publish 3306:3306 --name some-mysql --env MYSQL_ROOT_PASSWORD=admin --detach mysql:8.0.43-debian
```
📘**Nota** Los dos comandos anteriores son equivalentes

## Ejecute el comando /bin/bash en el contenedor de MySQL
```
docker exec -it some-mysql /bin/bash
```

### Dentro del contenedor ejecute MySQL

```
mysql -uroot -padmin demo
```

o simplemente:  

```
mysql -uroot -p
```

🔖Salga de MySQL con **exit** y también del contenedor con **exit**  

### Conexión desde equipo host

<details>
  <summary>Requiere mysql-client</summary>
  <pre>
    Si no tiene instalado el cliente de MySQL debe instalarlo con los comandos siguientes:
    sudo apt update
    sudo apt install -y mysql-client
  </pre>
</details>

```
mysql --host=127.0.0.1 --port=3306 --user=root --password=admin demo
```

## Inspeccionar un volumen
```
docker volume inspect myvolume
```

## Eliminar un volumen

```
docker volume rm myvolume
```

