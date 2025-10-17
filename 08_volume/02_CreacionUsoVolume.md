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

## 4. Ejecutar un contenedor de Alpine que utilice el volumen 📦myvolume

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume alpine
```

Explicación del comando:  

<table>
  <tr>
    <th>Parámetros</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>type</td>
    <td>Define el tipo de volumen a montar. 
    <ul>
      <li>type=volume, Montar un volumen</li>
      <li>type=bind, Montar un directorio del equipo host en el contenedor</li>
      <li>type=tmpfs, Montar un sistema de archivos temporal en memoria RAM (no es persistente)</li>
    </ul>
    </td>
  </tr>
  <tr>
    <td>src</td>
    <td>Deriva de la palabra source (origen) y se debe asingar el nombre del volumen que quiere usar</td>
  </tr>
  <tr>
    <td>dst</td>
    <td>Deriva de la palabra destination (destino) y se debe asignar la ruta donde se montará el volumen internamente en el contenedor. El directorio es creado automáticamente dentro del cotenedor</td>
  </tr>
</table>

## 5. Ejecutar un contenedor de Ubuntu que utilice el volumen 📦myvolume

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume ubuntu
```


```bash
docker run -it --rm -v myvolume:/myvolume ubuntu
```

***📘 Nota*** Los dos comandos anteriores tienen el mismo significado (son equivalentes).  


## 6. Ejecuta un contenedor de Ubuntu que utiliza 📦myvolume2

```
docker run -it --rm -v myvolume2:/data ubuntu bash
```
📚 Debido a que **myvolume2** es de tipo **tmpfs** los datos no son persistentes.  

## 7. Investigar dónde guarda las bases de datos MySQL

⚡Esto se hará para buscar el archivo **my.cnf** y la ruta de la carpeta **datadir** de **MySQL**

### Ejecutar un contenedor de mysql:8.0.43-debian en segundo plano sin utilizar volumen

```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```

#### Ejecutar el comando /bin/bash en el contenedor mysql:8.0.43-debian que está corriendo en segundo plano

Con este comando vamos a entar a la distribución de Linux que tiene instalado MySQL.

```
docker exec -it some-mysql /bin/bash
```

#### Buscando el archivo my.cnf

Ya dentro de la distribución de Linux, buscamos el archivo my.cnf

```
find / -name my.cnf
```
📖El archivo ***my.cnf** fue localizado en ***/etc/mysql/my.cnf***

#### Ver el contenido del archiov my.cnf

```
cat /etc/mysql/my.cnf
```

Allí está la línea que indica dónde se guardan las bases de datos ***datadir         = /var/lib/mysql***

#### Salga del contenedor mysql:8.0.43-debian

```
exit
```

### Detenga el contenedor de mysql:8.0.43-debian

```
docker container stop some-mysql
```

### Elimine el contenedor de mysql:8.0.43-debian

```
docker container rm some-mysql
```

## 8. Ejecute nuevamente el contenedor mysql:8.0.43-debian en segundo plano y usando el volumen 📦 myvolume

MySQL será expuesto al exterior en el puerto 3306. Esto servirá para conectarse a MySQL desde afuera del contenedor.

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

