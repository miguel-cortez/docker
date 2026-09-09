# Creación y uso de volúmenes

## 1. Crear un volumen

Se creará un volumen llamado 📦**myvolume**. Utilice solo uno de los dos comandos porque tienen el mismo significado (son equivalentes).  

```bash
docker volume create myvolume
```
```bash
docker volume create --name myvolume
```

## 2. Crear un volumen en memoria RAM  

Se creará un volumen llamado 📦**myvolume2**. Debido a que **myvolume2** es de tipo **tmpfs** los datos no son persistentes. 

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
    <td>-it</td>
    <td>El contenedor se ejecutará de forma interactiva. Equivale a --interactive.</td>
  </tr>
  <tr>
    <td>--mount</td>
    <td>Montar un volumen, directorio  o volumen en memoria RAM</td>
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

***📘 Nota*** Los dos comandos tienen el mismo significado (son equivalentes), por lo tanto, solo ejecute uno de los dos.   

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume ubuntu
```


```bash
docker run -it --rm -v myvolume:/myvolume ubuntu
```

## 6. Ejecutar un contenedor de Ubuntu que utilice el volumen 📦myvolume2

⚠️ Debido a que **myvolume2** es de tipo **tmpfs** los datos generados dentro del contenedor no son persistentes (se perderán al detener el contenedor).  

```
docker run -it --rm -v myvolume2:/data ubuntu bash
```

## 7. Investigar dónde se guardan las bases de datos MySQL

🔎 Esto se hará para buscar un archivo llamado 🗒️ **my.cnf** que tiene configuraciones de **MySQL**. Dentro de este archivo vamos a buscar una variable llamada **datadir** porque en ella está la ruta absoluta de donde se guardan las bases de datos.  

### a) Ejecutar un contenedor de mysql:8.0.43-debian en segundo plano sin utilizar volumen

```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```

### b) Ejecutar el comando /bin/bash en el contenedor mysql:8.0.43-debian que está corriendo en segundo plano

Con este comando vamos a entar a la distribución de Linux que tiene instalado MySQL.

```
docker exec -it some-mysql /bin/bash
```

### c) Buscando el archivo my.cnf

Ya dentro de la distribución de Linux, buscamos el archivo my.cnf

```
find / -name my.cnf
```
📖 El archivo **my.cnf** fue localizado en ***/etc/mysql/my.cnf***

### d) Ver el contenido del archiov my.cnf

```
cat /etc/mysql/my.cnf
```

Allí está la línea que indica dónde se guardan las bases de datos ***datadir         = /var/lib/mysql***

### e) Salga del contenedor mysql:8.0.43-debian

```
exit
```

### f) Detenga el contenedor de mysql:8.0.43-debian

```
docker container stop some-mysql
```

### g) Elimine el contenedor de mysql:8.0.43-debian

```
docker container rm some-mysql
```

## 8. Ejecute nuevamente el contenedor mysql:8.0.43-debian en segundo plano y usando el volumen 📦 myvolume

MySQL será expuesto al exterior en el puerto 3306. Esto servirá para conectarse a MySQL desde afuera del contenedor (desde Ubuntu que se instaló con WSL2 por ejemplo). ℹ️ Ejecute solo uno de los dos comandos siguientes porque tienen el mismo significado.  

```
docker run -v myvolume:/var/lib/mysql -p 3306:3306 --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```

```
docker run --mount type=volume,src=myvolume,dst=/var/lib/mysql --publish 3306:3306 --name some-mysql --env MYSQL_ROOT_PASSWORD=admin --detach mysql:8.0.43-debian
```

## 9. Cree una base de datos en contenedor de mysql:8.0.43-debian

### a) Ejecute el comando /bin/bash en el cotenedor de mysql:8.0.43-debian

Con esto ingresará la consola del contenedor que se está ejecutando en segundo plano.

```
docker exec -it some-mysql /bin/bash
```

### b) Dentro del contenedor de mysql:8.0.43-debian ingrese a MySQL

Solo use uno de los siguientes dos comandos:  

```
mysql -uroot -padmin
```

```
mysql -uroot -p
```

### c) Cree una base de datos llamada demo

```
create database demo;
```

### d) Salga de MySQL

```
exit;
```

### e) Salga del contenedor

```
exit
```

### 10. Conexión desde equipo host (desde Ubuntu instalado con WSL2)

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

## 11. Inspeccionar un volumen
```
docker volume inspect myvolume
```

## 12. Eliminar un volumen

ℹ️ Para poder eliminar un volumen, este no debe estar siendo utilizado por algún contenedor.  

```
docker volume rm myvolume
docker volume rm myvolume2
```

