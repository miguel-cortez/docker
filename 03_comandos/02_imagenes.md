# Comandos para trabajar con imágenes

## docker image load  
***Descripción***  
Carga una imagen de un archivo tar o STDIN.  

***Sintaxis***  
```
docker image load [OPTIONS]
```

***Alias***  
```
docker load
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-i, --input</td>
    <td>Leer desde el archivo tar, en lugar de STDIN.</td>
  </tr>
  <tr>
    <td>-q, --quiet	</td>
    <td>Suprimir la salida de carga</td>
  </tr>
</table>  

[Más opciones](https://docs.docker.com/reference/cli/docker/image/load/)  

## docker image ls

***Descripción***  
Lista las imágenes.  

***Sintaxis***  
```
docker image ls [OPTIONS] [REPOSITORY[:TAG]]
```

***Alias***  
```
docker image list, docker images
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-a, --all</td>
    <td>Mostrar todas las imágenes (por defecto se ocultan las imágenes intermedias)</td>
  </tr>
  <tr>
    <td>-q, --quiet	</td>
    <td>Mostrar únicamente los IDs de imágenes</td>
  </tr>
</table>  

[Más opciones](https://docs.docker.com/reference/cli/docker/image/ls/)  

## docker image pull  
***Descripción***  
Descarga una imagen desde un registro.  

***Sintaxis***  
```
docker image pull [OPTIONS] NAME[:TAG|@DIGEST]
```

***Alias***  
```
docker pull
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-a, --all-tags</td>
    <td>Descargar todas las imágenes etiquetadas en el repositorio</td>
  </tr>
  <tr>
    <td>-q, --quiet	</td>
    <td>Suprimir salida detallada</td>
  </tr>
</table>

[Más opciones](https://docs.docker.com/reference/cli/docker/image/pull/)  

## docker image push  
***Descripción***  
Subir una imagen a un registro.  

***Sintaxis***  
```
docker image push [OPTIONS] NAME[:TAG]
```

***Alias***  
```
docker push
```

[Más información](https://docs.docker.com/reference/cli/docker/image/push/)  

## docker image rm  

***Descripción***  
Elimina una o más imágenes.  

***Sintaxis***  
```
docker image rm [OPTIONS] IMAGE [IMAGE...]
```

***Alias***  
```
docker image remove, docker rmi
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-f, --force</td>
    <td>Forzar la eliminación de la imagen</td>
  </tr>
</table>

[Más opciones](https://docs.docker.com/reference/cli/docker/image/rm/)  

## docker image save  

***Descripción***  
Guardar una o más imágenes en archivo .tar (streamed to STDOUT by default).  

***Sintaxis***  
```
docker image save [OPTIONS] IMAGE [IMAGE...]
```

***Alias***  
```
docker save
```

[Más opciones](https://docs.docker.com/reference/cli/docker/image/save/)  

## docker image tag  

***Descripción***  
Crea una etiqueta TARGET_IMAGE que haga referencia a SOURCE_IMAGE.    

***Sintaxis***  
```
docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

***Alias***  
```
docker tag
```

Una referencia de imagen de Docker consta de varios componentes que describen dónde se almacena la imagen y su identidad. Estos componentes son:
```
[HOST[:PORT]/]NAMESPACE/REPOSITORY[:TAG]
```

**HOST**  

Especifica la ubicación del registro donde reside la imagen. Si se omite, Docker utiliza de forma predeterminada Docker Hub (docker.io).  

**PORT**  

Un número de puerto opcional para el registro, si es necesario (por ejemplo, :5000).  

**NAMESPACE/REPOSITORY**  

El espacio de nombres (opcional) suele representar a un usuario o una organización. El repositorio es obligatorio e identifica la imagen específica. Si se omite el espacio de nombres, Docker utiliza por defecto library, el espacio de nombres reservado para las imágenes oficiales de Docker.  

**TAG**  

Identificador opcional utilizado para especificar una versión o variante concreta de la imagen. Si no se proporciona ninguna etiqueta, Docker utiliza por defecto la última versión.  

[Más información](https://docs.docker.com/reference/cli/docker/image/tag/)  

## Referencia

[Imágenes de docker](https://docs.docker.com/reference/cli/docker/image/)  
