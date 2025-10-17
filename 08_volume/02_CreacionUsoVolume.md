# Creación y uso de volúmenes

## Crear un volumen

```bash
docker volume create --name myvolume
docker volume create myvolume
```
**Crear un volumen en memoria RAM**  

```
docker volume create --driver local --opt type=tmpfs –opt device=tmpfs --opt o=size=100m,uid=1000 myvolume2
```

## Listar los volúmenes
```bash
docker volume ls
```

## Utilizar un volumen

**Ejecuta un contenedor de alpine**  

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


**Ejecuta un contenedor de ubuntu**  

***Nota*** Los dos comandos tienen el mismo significado (son equivalentes).  

```bash
docker run -it --rm --mount type=volume,src=myvolume,dst=/myvolume ubuntu
docker run -it --rm -v myvolume:/myvolume ubuntu
```

**Ejecuta un contenedor que utiliza myvolume2**

```
docker run -it --rm -v myvolume2:/data ubuntu bash
```
📚 Debido a que **myvolume2** es de tipo **tmpfs** los datos no son persistentes.  

**Ejecuta un contenedor de MySQL**  

```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```

**Ejecutar un comando en un contenedor que está corriendo en segundo plano**  

```
docker exec -it some-mysql /bin/bash
```

Buscando el archivo my.cnf  
```
find / -name my.cnf
```

Ver el contenido del archiov my.cnf  

```
cat /etc/mysql/my.cnf
```

**Salga del contenedor**  

```
exit
```

**Detenga el contenedor que se está ejecutando en segundo plano**  

```
docker container rm some-mysql
```

**Elimine el contenedor que detuvo recientemente**  

```
docker container rm some-mysql
```

**Ejecutar nuevamente mysql; pero usando el volumen myvolume**  

```
docker run -v myvolume:/var/lib/mysql --name some-mysql -e MYSQL_ROOT_PASSWORD=admin -d mysql:8.0.43-debian
```
**Ejecutar un segundo comando en el contenedor**
```
docker exec -it some-mysql /bin/bash
```

**Dentro del contenedor ejecute MySQL**  

```
mysql -uroot -padmin demo
```

o simplemente:  
```
mysql -uroot -p
```

**Conexión desde equipo host**  

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

