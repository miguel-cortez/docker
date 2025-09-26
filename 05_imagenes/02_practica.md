# Imágenes personalizadas con Docker

## ¿Qué es Dockerfile?

Docker puede crear imágenes automáticamente leyendo las instrucciones de un archivo Dockerfile. Un archivo Dockerfile es un documento de texto que contiene todos los comandos que un usuario podría ejecutar en la línea de comandos para ensamblar una imagen.

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

## Pasos generales para crear una imagen personalizada
1. Crear una carpeta con un nombre descriptivo.
2. Ingresar a la carpeta recién creada.
3. Crear una archivo llamado `Dockerfile` dentro de la carpeta.
4. Editar y guardar los cambios en el archivo `Dockerfile`
5. Construir la imagen con el comando `docker build` 

## Ejemplos

## Ejemplo 1

***Descripción**

Crear una imagen personalizada basada en la distribución `ubuntu:24.04`, actualizar la lista de paquetes disponibles e instalar el paquete `mc`   

```
FROM ubuntu:24.04
RUN apt update && apt install -y mc
```

## Ejemplo 2

***Descripción**

Crear una imagen personalizada basada en la distribución `ubuntu:24.04`, actualizar la lista de paquetes disponibles e instalar el paquete `vin`. Ejecutar de forma predeterminada el comando `bash`   

```
FROM ubuntu:24.04
RUN apt update && apt install -y vim
CMD ["bash"]
```

## Ejemplo 3

***Descripción**

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
echo "Hola Miguel"
```
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
<img width="779" height="169" alt="imagen" src="https://github.com/user-attachments/assets/af3097d9-6112-4b16-8e08-6c715982dc64" />

[Referencia de Dockerfile](https://docs.docker.com/reference/dockerfile/)  
