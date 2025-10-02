# Dockerizar un proyecto de Laravel

## .Gitignore

```
# Archivos del sistema operativo
.DS_Store
Thumbs.db
.vscode/
.idea/

# Archivos de Git
.git
.gitignore

# Archivos de Laravel
/node_modules
/vendor
/storage/*.key
/storage/debugbar
.env
.env.*    # Excluir archivos de entorno de desarrollo
.phpunit.result.cache
/public/build
/public/hot

# npm / Vue
npm-debug.log*
yarn-error.log*
dist/
*.log
# Caché / Artefactos de compilación

*.cache
*.tmp
*.swp

```
### Explicaciones

**Sistema operativo**  

.DS_Store / Thumbs.db: Archivos generados automáticamente por macOS y Windows, respectivamente. Innecesarios para la imagen.

.vscode/ / .idea/: Directorios de configuración de editores (VS Code, PhpStorm). Son personales y no deben incluirse en la imagen.

### Git

.git: El historial de Git no es necesario dentro de una imagen de Docker.

.gitignore: Es solo relevante para Git, no tiene uso dentro del contenedor.

### Laravel

/node_modules: Las dependencias de frontend no se deberían copiar (Docker debería hacer npm install en el contenedor si se necesita).

/vendor: Lo mismo para dependencias PHP (Composer).

/storage/*.key: Podría incluir claves privadas (ej. para cifrado).

/storage/debugbar: Archivos generados por Laravel Debugbar. Solo útiles para desarrollo.

.env / .env.*: Variables sensibles (bases de datos, claves API, etc). Importante NO copiarlas a la imagen.

.phpunit.result.cache: Cache de PHPUnit.

### npm / Vue

Logs (*.log, npm-debug.log*, yarn-error.log*): Archivos de error que no aportan valor en una imagen.

dist/: Carpeta típica de builds frontend. Si se usa un proceso de build dentro de Docker, no es necesario copiarla desde fuera.

### Caché / Artefactos de compilación

Caché, archivos temporales y de swap (.swp): Archivos generados por editores, herramientas de desarrollo, etc. Innecesarios.

### Recomendaciones (opcionales)

Si vas a usar composer install dentro del Dockerfile, excluir /vendor es correcto. Pero si lo haces fuera (en la máquina host), necesitarás copiar /vendor (por lo tanto deberías eliminarlo del .dockerignore en ese caso).

Lo mismo aplica para /node_modules y dist/.

