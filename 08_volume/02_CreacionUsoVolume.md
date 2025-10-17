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

## Inspeccionar un volumen
```
docker volume inspect myvolume
```

## Eliminar un volumen

```
docker volume rm myvolume
```

