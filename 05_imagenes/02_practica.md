# Imágenes personalizadas con Dockerfile

## ¿Qué es Dockerfile?

- Docker puede crear imágenes automáticamente leyendo las instrucciones de un archivo Dockerfile.
- Un archivo Dockerfile es un documento de texto que contiene todos los comandos que un usuario podría ejecutar en la línea de comandos para ensamblar una imagen.

<table>
  <tr>
    <th>Instrucción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>CMD</td>
    <td>Especifica comandos predeterminados del contenedor</td>
  </tr>
  <tr>
    <td>COPY</td>
    <td>Copia archivos y directorios</td>
  <tr>
    <td>FROM</td>
    <td>Crea una nueva etapa de compilación a partir de una imagen base</td>
  </tr>
  <tr>
    <td>RUN</td>
    <td>Ejecuta comandos de compilación de la imagen</td>
  </tr>
  <tr>
    <td>WORKDIR</td>
    <td>Cambia el directorio de trabajo</td>
  </tr>
</table>

## ℹ️ Pasos generales para crear una imagen personalizada
1. Crear una carpeta con un nombre descriptivo.
2. Ingresar a la carpeta recién creada.
3. Crear una archivo llamado `Dockerfile` dentro de la carpeta.
4. Editar y guardar los cambios en el archivo `Dockerfile`
5. Construir la imagen con el comando `docker build` 

## 🔰Ejemplos

## Ejemplo 1

***Descripción***

Crear una imagen personalizada basada en la distribución `ubuntu:24.04`, actualizar la lista de paquetes disponibles e instalar el paquete `mc`   

```
FROM ubuntu:24.04
RUN apt update && apt install -y mc
```

## Ejemplo 2

***Descripción***

Crear una imagen personalizada basada en la distribución `ubuntu:24.04`, actualizar la lista de paquetes disponibles e instalar el paquete `vin`. Ejecutar de forma predeterminada el comando `bash`   

```
FROM ubuntu:24.04
RUN apt update && apt install -y vim
CMD ["bash"]
```

## Ejemplo 3

***Descripción***

Crear una imagen personalizada basada en la distribución `busybox` y ejecutar un script de Bash (.sh)  

### 1. Crear una carpeta llamada busybox_sh

```
mkdir busybox_sh
```

### 2. Ingrese a la carpeta busybox_sh

```
cd busybox_sh
```

### 3. Cree un archivo llamado hello.sh y escriba su contenido

```
#!/bin/sh
echo "Hola Miguel Cortez"

for i in 1 2 3 4 5; do
  echo "Welcome $i times"
done
```
📑Nota. No vaya a escribir **Miguel Cortez** sino su nombre (nombre completo es mejor).  

### 4. Cree un archivo llamado Dockerfile y escriba su contenido

```
FROM busybox
COPY /hello.sh /
RUN chmod 777 /hello.sh
RUN  sh /hello.sh #Corre cuando se crea la imagen.
CMD["./hello.sh"]
```

### 5. Ejecute el comando para crear la imagen personalizada

```
docker image build -t busybox_sh:v1 .
```

### 6. Cree y ejecute un contenedor a partir de la imagen personalizada

```
docker container run --rm busybox_sh:v1
```
<img width="793" height="162" alt="imagen" src="https://github.com/user-attachments/assets/8186eb5a-0e54-4bd9-8950-2f53f210ad18" />


### 📚 Notas adicionales
***Listar las imágenes***

<img width="647" height="116" alt="imagen" src="https://github.com/user-attachments/assets/31427591-3c17-4d7d-9193-235594ad6622" />

***Eliminar una imagen***
```
sudo docker rmi f6c21906ea0c
```

Donde `f6c21906ea0c` es el `IMAGE ID` de la imagen que quiere borrar.  

⚠️ **¿Quiere borrar una imagen?**. Algunas veces no se puede borrar porque tiene un contenedor asociado. Primero debe eliminar el contenedor y luego podrá eliminar la imagen.

***Eliminar un contenedor***
```
sudo docker container rm 8c4c04566d4d
```
Donde `8c4c04566d4d` es el `CONTAINER ID` del contenedor que quiere borrar.  

***Listar los contenedores***  

<img width="1632" height="120" alt="imagen" src="https://github.com/user-attachments/assets/d07bf91a-4a50-4f0f-bb18-a40a4d4af1d5" />


[Referencia de Dockerfile](https://docs.docker.com/reference/dockerfile/)  
