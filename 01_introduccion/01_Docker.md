# Docker

## ¿Qué es Docker?
Docker es una plataforma de código abierto para crear y gestionar aplicaciones en contenedores, que son unidades estandarizadas que empaquetan el código de una aplicación con sus dependencias (bibliotecas, código, herramientas del sistema) para que pueda ejecutarse de forma fiable en cualquier entorno. Su uso simplifica el desarrollo y despliegue de aplicaciones, garantizando que funcionen de la misma manera desde el entorno local hasta la nube, y ofrece ventajas como la modularidad, la velocidad y la portabilidad.  

## Conceptos

### 1. Contenedor

Docker agrupa las aplicaciones y sus dependencias en contenedores, que son aislados unos de otros pero comparten el mismo kernel del sistema operativo host, lo que los hace más ligeros que las máquinas virtuales tradicionales

### 2. Imagen
Una imagen es una plantilla que contiene todos los elementos necesarios para crear un contenedor. Las imágenes son inmutables y pueden ser compartidas, facilitando la replicación y la distribución  

### 3. Docker Engine
Es el software que se instala en un servidor y que se encarga de construir, ejecutar y gestionar los contenedores.  

### 4. Dockerfile
Es un archivo de texto que contiene las instrucciones para construir una imagen de Docker, definiendo cómo se debe configurar el contenedor

## Docker vs Virtualización

<img width="3840" height="1332" alt="SWTM-2060_Diagram_Containers_VirtualMachines_v03" src="https://github.com/user-attachments/assets/b852ec12-dc2e-4b32-aed7-5cff9fa97338" />

### Tabla comparativa Docker vs Máquina virtual

<img width="804" height="340" alt="imagen" src="https://github.com/user-attachments/assets/fa5b2f34-bdf5-4815-9451-f19aab81c312" />

### Imagen vs Contenedor

**Imagen**, Una imagen es una plantilla, de un contenedor que corre alguna aplicación. Las imágenes al ser plantillas van a ser  usadas para crear nuevos contenedores. Las plantillas nunca cambian a menos que creemos una nueva a partir de un contenedor.

**Contenedor**, Consideramos instancia a la ejecución de una imagen. Las instancias son las encargadas de ejecutar las aplicaciones que necesitamos. A partir de una única imagen, podemos ejecutar varios contenedores a los que también llamaremos instancias.

<img width="1400" height="466" alt="como-funciona-docker" src="https://github.com/user-attachments/assets/b8f686a4-cc65-4503-9159-bdecb18b61e2" />

Múltiples contenedores a partir de una misma imagen  

<img width="290" height="173" alt="docker_imagenes_contenedores" src="https://github.com/user-attachments/assets/1754c4f9-c3dd-4a2e-bc27-afde4eb9ed1e" />
