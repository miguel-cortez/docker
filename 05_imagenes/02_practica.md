# Imágenes personalizadas

## 1. Crear una carpeta llamada busybox_sh

```
mkdir busybox_sh
```

## 2. Ingrese a la carpeta busybox_sh

```
cd busybox_sh
```

## 3. Cree un archivo llamado hello.sh y escriba su contenido

```
#!/bin/sh
echo "Hola Miguel"
```
## 4. Cree un archivo llamado Dockerfile y escriba su contenido

```
FROM busybox
COPY /hello.sh /
RUN chmod 777 /hello.sh
RUN  sh /hello.sh #Corre cuando se crea la imagen.
CMD["./hello.sh"]
```

## 5. Ejecute el comando para crear la imagen personalizada

```
docker image build -t busybox_sh:v1 .
```

## 6. Cree y ejecute un contenedor a partir de la imagen personalizada

```
docker container run --rm busybox_sh:v1
```
<img width="779" height="169" alt="imagen" src="https://github.com/user-attachments/assets/af3097d9-6112-4b16-8e08-6c715982dc64" />

