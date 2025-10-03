# Imagen personalizada para un proyecto de Laravel

## 1. Crear un archivo .dockerignore

📓 **Notas**.
- En el archivo **.dockerignore** se deben especificar los directorios y archivos que no serán incluidos en la imagen personalizada de Docker.
- Crear un archivo **.dockerignore** requiere del conocimiento del **framework** que se está utilizando para saber qué directorios y qué archivos son necesarios en tiempo de ejecución.
- Incluir directorios y archivos innecesarios para la ejecución de la aplicación solo hacen más pesada la imagen.
- En Internet puede encontrar el contenido sugerido para un archivo **.dockerignore** para proyectos creados con diferentes tecnologías y lenguajes de programación.

***Contenido del archivo .dockerignore para una aplicación de Laravel***  

```bash
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
### Descripción del contenido incluido en .dockerignore

***Sistema operativo***  
- **.DS_Store / Thumbs.db**: Archivos generados automáticamente por macOS y Windows, respectivamente. Innecesarios para la imagen.  
- **.vscode/** / **.idea/**: Directorios de configuración de editores (VS Code, PhpStorm). Son personales y no deben incluirse en la imagen.  

***Git***  
- **.git**: El historial de Git no es necesario dentro de una imagen de Docker.  
- **.gitignore**: Es solo relevante para Git, no tiene uso dentro del contenedor.  

***Laravel***  
- **/node_modules**: Las dependencias de frontend no se deberían copiar (Docker debería hacer npm install en el contenedor si se necesita).  
- **/vendor**: Lo mismo para dependencias PHP (Composer).  
- **/storage/*.key**: Podría incluir claves privadas (ej. para cifrado).  
- **/storage/debugbar**: Archivos generados por Laravel Debugbar. Solo útiles para desarrollo.  
- **.env** / **.env.***: Variables sensibles (bases de datos, claves API, etc). Importante NO copiarlas a la imagen.  
- **.phpunit.result.cache**: Cache de PHPUnit.  

***npm / Vue***  
- **Logs (*.log, npm-debug.log*, yarn-error.log*)**: Archivos de error que no aportan valor en una imagen.  
- **dist/**: Carpeta típica de builds frontend. Si se usa un proceso de build dentro de Docker, no es necesario copiarla desde fuera.  

***Caché / Artefactos de compilación***  
- **Caché, archivos temporales y de swap (.swp)**: Archivos generados por editores, herramientas de desarrollo, etc. Innecesarios.  

***Recomendaciones (opcionales)***  
- Si se va a usar ***composer install*** dentro del **Dockerfile**, es correcto excluir ***/vendor***. Pero si se hace fuera (en la máquina host), se necesita copiar ***/vendor*** (por lo tanto debería ser eliminarlo del archivo **.dockerignore**). Lo mismo aplica para **/node_modules** y **dist/**.  

## 2. Crear un archivo Dockerfile

```bash
# Imagen base para crear la imagen personalizada
FROM php:8.3-fpm-alpine

# Instalación de paquetes necesarios
RUN apk add --no-cache \
    bash \
    curl \
    unzip \
    git \
    zip \
    libpng-dev \
    libxml2-dev \
    oniguruma-dev \
    build-base \
    nodejs \
    npm \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS

# Instalación de extensiones de PHP
RUN docker-php-ext-install pdo pdo_mysql mbstring exif pcntl bcmath gd \
    && docker-php-source delete \
    && apk del .build-deps

# Instalación de composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copiar el código de la aplicación al directorio /var/www de la imagen personalizada
WORKDIR /var/www
COPY . .

# Instalación de Composer (producción) y eliminación de archivos innecesarios
RUN composer install --optimize-autoloader --no-dev --prefer-source

# Compilación de Node.js y generación de código optimizado para producción
RUN npm install && npm run build

# Establecer permisos para Laravel
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache

# Comandos de inicio de la aplicación Laravel
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
```

# 3. Creación de la imagen personalizada
```bash
docker image build -t example-app:v1.0 .
```
# 4. Ejeución de la aplicación

```bash
docker container run --rm \
  --name example-app \
  -e APP_NAME=Laravel \
  -e APP_ENV=local \
  -e APP_KEY=base64:JNDOAJ1EjY9aIN2YqADPn0IcIpIFX1JYl0Sp00G04lc= \
  -e APP_DEBUG=true \
  -e APP_URL=http://localhost \
  -e APP_LOCALE=en \
  -e APP_FALLBACK_LOCALE=en \
  -e APP_FAKER_LOCALE=en_US \
  -e APP_MAINTENANCE_DRIVER=file \
  -e PHP_CLI_SERVER_WORKERS=4 \
  -e BCRYPT_ROUNDS=12 \
  -e LOG_CHANNEL=stack \
  -e LOG_STACK=single \
  -e LOG_DEPRECATIONS_CHANNEL=null \
  -e LOG_LEVEL=debug \
  -e DB_CONNECTION=mysql \
  -e DB_HOST=127.0.0.1 \
  -e DB_PORT=3306 \
  -e DB_DATABASE=example_app \
  -e DB_USERNAME=root \
  -e DB_PASSWORD= \
  -e SESSION_DRIVER=database \
  -e SESSION_LIFETIME=120 \
  -e SESSION_ENCRYPT=false \
  -e SESSION_PATH=/ \
  -e SESSION_DOMAIN=null \
  -e BROADCAST_CONNECTION=log \
  -e FILESYSTEM_DISK=local \
  -e QUEUE_CONNECTION=database \
  -e CACHE_STORE=database \
  -e MEMCACHED_HOST=127.0.0.1 \
  -e REDIS_CLIENT=phpredis \
  -e REDIS_HOST=127.0.0.1 \
  -e REDIS_PASSWORD=null \
  -e REDIS_PORT=6379 \
  -e MAIL_MAILER=log \
  -e MAIL_SCHEME=null \
  -e MAIL_HOST=127.0.0.1 \
  -e MAIL_PORT=2525 \
  -e MAIL_USERNAME=null \
  -e MAIL_PASSWORD=null \
  -e MAIL_FROM_ADDRESS="hello@example.com" \
  -e MAIL_FROM_NAME="${APP_NAME}" \
  -e AWS_ACCESS_KEY_ID= \
  -e AWS_SECRET_ACCESS_KEY= \
  -e AWS_DEFAULT_REGION=us-east-1 \
  -e AWS_BUCKET= \
  -e AWS_USE_PATH_STYLE_ENDPOINT=false \
  -e VITE_APP_NAME="${APP_NAME}" \
  -p 8000:8000 \
  example-app:v1.0
```
