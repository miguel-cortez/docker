# Docker  

## ¿Qué es Docker?  
- Docker es una plataforma de código abierto para crear y gestionar aplicaciones en contenedores, que son unidades estandarizadas que empaquetan el código de una aplicación con sus dependencias (bibliotecas, código, herramientas del sistema) para que pueda ejecutarse de forma fiable en cualquier entorno.
- Su uso simplifica el desarrollo y despliegue de aplicaciones, garantizando que funcionen de la misma manera desde el entorno local hasta la nube, y ofrece ventajas como la modularidad, la velocidad y la portabilidad.  

## ¿Docker es nativo de Windows o Linux?  
Docker se creó originalmente sobre Linux y funciona de manera nativa allí al aprovechar tecnologías como los grupos de control y espacios de nombres del kernel de Linux. Para Windows, en cambio, Docker utiliza una máquina virtual o el subsistema de Windows para Linux (WSL) para poder ejecutar sus contenedores, los cuales se basan fundamentalmente en Linux.

## ¿Cómo funciona Docker en Windows?  

***Máquina virtual o WSL***, Para ejecutar contenedores en Windows, Docker utiliza un sistema subyacente. Históricamente esto ha sido una máquina virtual, pero ahora se usa el Subsistema de Windows para Linux (WSL 2)  

<img width="600" alt="wsl-en" src="https://github.com/user-attachments/assets/5f21744c-9e87-4b69-8578-a4a4578af5b9" />

***Docker Desktop***, En Windows, se suele usar la aplicación Docker Desktop, que proporciona una integración conveniente y permite ejecutar tanto contenedores de Linux como de Windows en la misma máquina, aunque no simultáneamente.  

## Requerimientos de WSL 2
Para utilizar WSL 2, necesitas Windows 10 versión 2004 (compilación 19041) o superior, o Windows 11. Es imprescindible un procesador de 64 bits con traducción de direcciones de segundo nivel (SLAT) y tener la virtualización de hardware habilitada en la BIOS/UEFI de tu equipo. Además, se requiere una cantidad adecuada de memoria RAM (mínimo 4 GB)

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

### Tipos de Hypervisores  

<img width="838" height="477" alt="imagen" src="https://github.com/user-attachments/assets/e39e5a46-71b4-49b8-a68d-eb9126a2843c" />

***Tipo 1***  
- Se ejecuta directamente sobre el hardware  
- Ofrece mejor rendimiento y eficiencia
- Es ideal para entornos empresariales
- Requiere configuración más compleja
- Tiene mayor seguridad al tener menos capas

***Tipo 2***  
- Corre sobre un sistema operativo
- Es más lento por la capa adicional del sistema operativo
- Se utiliza más en pruebas y uso personal
- Es más sencillo de instalar y usar
- Depende de la seguridad del sistema operativo host  

### Tabla comparativa Docker vs Máquina virtual

<table>
  <tr>
    <th>Características</th>
    <th>Docker (contenedores)</th>
    <th>Vritualización(máquinas virtuales)</th>
  </tr>
  <tr>
    <td>Aislamiento</td>
    <td>Comparte el kernel dis SO host</td>
    <td>Aislamiento completo con OS propio</td>
  </tr>
  <tr>
    <td>Uso de recursos</td>
    <td>Muy ligero (menos CPU, MEMORIA y disco)</td>
    <td>Mayor consumo de recursos</td>
  </tr>
  <tr>
    <td>Tiempo de inicio</td>
    <td>Muy rápido (segundos)</td>
    <td>Lento (puede tardar varios minutos)</td>
  </tr>
  <tr>
    <td>Portabilidad</td>
    <td>Muy alta, ejecutable en cualquier sistema con Docker</td>
    <td>Limitada, depende del hipervisor y configuraciones</td>
  </tr>
  <tr>
    <td>Desempeño</td>
    <td>Casi nativo, sin sobrecarga de hipervisor</td>
    <td>Más lento por la capa de virtualización</td>
  </tr>
  <tr>
    <td>Seguridad</td>
    <td>Aislamiento limitado(riesgos si se compromete el host)</td>
    <td>Mayor aislamiento, más seguro en entornos críticos</td>
  </tr>
</table>

### Imagen vs Contenedor

**Imagen**, Una imagen es una plantilla, de un contenedor que corre alguna aplicación. Las imágenes al ser plantillas van a ser  usadas para crear nuevos contenedores. Las plantillas nunca cambian a menos que creemos una nueva a partir de un contenedor.

**Contenedor**, Consideramos instancia a la ejecución de una imagen. Las instancias son las encargadas de ejecutar las aplicaciones que necesitamos. A partir de una única imagen, podemos ejecutar varios contenedores a los que también llamaremos instancias.

<img width="1400" height="466" alt="como-funciona-docker" src="https://github.com/user-attachments/assets/b8f686a4-cc65-4503-9159-bdecb18b61e2" />

Múltiples contenedores a partir de una misma imagen  

<img width="290" height="173" alt="docker_imagenes_contenedores" src="https://github.com/user-attachments/assets/1754c4f9-c3dd-4a2e-bc27-afde4eb9ed1e" />
