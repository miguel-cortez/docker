# Creación de una imagen personalizada
## hello.sh

```
#!/bin/sh
echo "Hola Miguel"
```
## Dockerfile

```
FROM busybox
COPY /hello.sh /
RUN chmod 777 /hello.sh
RUN  sh /hello.sh #Corre cuando se crea la imagen.
CMD["./hello.sh"]
```

## Creación de la imagen

```
docker image build -t busybox_sh:latest .
```

## Ejecución de un contenedor a partir de la imagen creada

```
 docker container run --rm busybox_sh:latest
```

