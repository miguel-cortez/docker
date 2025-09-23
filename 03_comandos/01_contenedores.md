# docker container run

***Descripción***  
Crea y ejecuta un nuevo contenedor desde una imagen.  

***Sintaxis***  
```
docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

***Alias***  
```
docker run
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-d, --detach</td>
    <td>Ejecuta el cotenendor en segundo plano e imprime el ID del contenedor</td>
  </tr>
  <tr>
    <td>-e, --env</td>
    <td>Establece variables de entorno</td>
  </tr>
  <tr>
    <td>--env-file</td>
    <td>Lee variables de entorno de un archivo</td>
  </tr>
  <tr>
    <td>--name</td>
    <td>Asigna nombre al contenedor</td>
  </tr>
  <tr>
    <td>-p, --publish</td>
    <td>Publica un puerto del contenedor a un puerto del host</td>
  </tr>
  <tr>
    <td>--rm</td>
    <td>Elimina automáticamente el contenedor y sus volúmenes asociados en caso que existan</td>
  </tr>
  <tr>
    <td>-t, --tty</td>
    <td>Localiza una terminal tty</td>
  </tr>
</table>

📗 **Nota**. En Linux, una TTY (teletype) es un dispositivo de caracteres que permite la entrada y salida de datos, generalmente para interactuar con el sistema operativo a través de un teclado y una pantalla.  

[Más opciones](https://docs.docker.com/reference/cli/docker/container/run/)  

# docker container exec  

***Descripción***  
Ejecuta un comando en un contenedor en ejecución.  

***Sintaxis***  

```
docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]
````

***Alias***  
```
docker exec
```

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-d, --detach</td>
    <td>Modo desatachado: Corre un comando en segundo plano</td>
  </tr>
  <tr>
    <td>-i, --interactive</td>
    <td>Se ejecuta el comando en modo interactivo</td>
  </tr>
  <tr>
    <td>-t, --tty</td>
    <td>Asigna una tty</td>
  </tr>
</table>

[Más opciones](https://docs.docker.com/reference/cli/docker/container/exec/)  

# docker container ls

***Descripción***  
Lista contenedores  

***Sintaxis***
```
docker container ls [OPTIONS]
```

***Alias***  
`docker container list`, `docker container ps`, `docker ps`  

<table>
  <tr>
    <th>Opciones</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td>-a, --all</td>
    <td>Muestra todos los contenedores (por defecto solo muestra los contenedores en ejecución)</td>
  </tr>
</table>

[Más opciones](https://docs.docker.com/reference/cli/docker/container/ls/)  

# docker container rm  

***Descripción***    
Elimina uno o más contenedores.  

***Sintaxis***  
```
docker container rm [OPTIONS] CONTAINER [CONTAINER...]
```

***Alias***  
`docker container remove`, `docker rm` 

[Más opciones](https://docs.docker.com/reference/cli/docker/container/rm/)  

# docker container stop  

***Descripción***  
Detiene uno o más contenedores en ejecución  

***Sintaxis***  
```
docker container stop [OPTIONS] CONTAINER [CONTAINER
```

***Alias***

```
docker stop
```

[Más opciones](https://docs.docker.com/reference/cli/docker/container/stop/)  






