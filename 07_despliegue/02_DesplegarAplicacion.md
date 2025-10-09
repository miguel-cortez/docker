# Desplegar una aplicación en Koyeb.com

## Paso 1. Inicie sesión en Koyeb.com

## Paso 2. Cree un nuevo servicio
<img width="1358" height="474" alt="imagen" src="https://github.com/user-attachments/assets/306c7b9c-6718-4d27-a8d3-fb9aed08db0c" />

### Importe el proyecto

En la parte derecha de **Image** debe escribir el nombre de la imagen que quiere importar desde **Docker Hub**. En el ejemplo se está importando la imagen **miguelcortez01/example_app:v1.0**. Koyeb determinará si la imagen está disponible, en tal caso, se mostrará el mensaje ***The image is accessible***  

<img width="1084" height="544" alt="imagen" src="https://github.com/user-attachments/assets/27d399f4-ffde-491b-86a3-17007e837fce" />


## Seleccione la instancia y región

<img width="1180" height="675" alt="imagen" src="https://github.com/user-attachments/assets/03f9161a-5e14-4133-9a78-7832097d5fd7" />

## Configure las variables de entorno y haga clic en Deploy 

<img width="1361" height="674" alt="imagen" src="https://github.com/user-attachments/assets/96cb48ba-c933-4c1a-a645-b9f564c7bad0" />

<img width="680" height="405" alt="imagen" src="https://github.com/user-attachments/assets/7e2cd678-0209-4876-acfb-54a31aff77fb" />

<img width="1364" height="655" alt="imagen" src="https://github.com/user-attachments/assets/08e941a5-b9d7-4fc0-af2e-29f87371e23b" />

<img width="1361" height="653" alt="imagen" src="https://github.com/user-attachments/assets/003605dd-0029-4920-aeae-a83f5beb2ef7" />



🤵**Identificación** La cuenta será habilitada por 7 días según las indicaciones dadas por la plataforma. Le peditará configurar su información personal mediante la captura de imágenes de un documento como el DUI y además, fotografía de su rostro. Para ver el contenido del sitio web publicado no necesariamente deberá enviar su información; personal pero es probable que después de 7 días sea obligatorio para disponer de la cuenta.  

## Listo. Pruebe la apliación en nagevador Web.

🔖***SIN ESTILOS CSS*** No presenta estilos CSS. Al ingresar a las ***Opciones de desarrollador*** se puede ver que tampoco carga los archivos JS.  

### Sitio web visto en Google Chrome**

<img width="1918" height="860" alt="imagen" src="https://github.com/user-attachments/assets/10a842d7-e9c9-4d2c-88c4-4a04eda6a02b" />


### Sitio web visto en Mozilla Firefox

<img width="1875" height="894" alt="imagen" src="https://github.com/user-attachments/assets/45f334b4-e00b-4f6c-b94e-173accdf0a3c" />



### Causas del error  
- Si los estilos CSS no se aplican al sitio web se puede deber a varias razones como: no se ha agregado al contenedor los archivos css y js, problemas de permisos en la carpeta public/build, etc.
- 🔑 En este caso el error se debe a que ***el sitio web publicado utiliza el protocolo https y los archivos css y js están tratando de cargar mediante el protocolo http***

🎲 Si el sitio web no se puede ver porque requiere **HTTPS** debe modificar el archivo **AppServiceProvider.php** de la aplicación.    

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Schema;
use Illuminate\Support\Facades\URL; // 👈 LÍNEA AGREGADA
class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        Schema::defaultStringLength(191);
        // ⏬ AGREGAR DESDE AQUÍ
        if (config('app.env') === 'production') {
            URL::forceScheme('https');
        }
        // 🔼 HASTA AQUÍ
    }
}
```

📚**Nota** Los cambios realizados en **AppServiceProvider.php** obligan a que en entorno de **producción** todos los recursos se carguen mediante el protocolo **https**  

Recuerde que es necesario craer una nueva imagen de Docker y volter a subir y publicar el contenido del sitio web.  

En koyeb:  

```bash
APP_DEBUG: false
APP_ENV: production
APP_URL: https://limited-kiri-macvorganization-5368fa94.koyeb.app/
LOG_LEVEL: info
SESSION_SECURE_COOKIE=true // ⚙ NUEVA VARIABLE
```


<img width="996" height="653" alt="imagen" src="https://github.com/user-attachments/assets/0ab16c05-a670-4e67-bb08-4fb75381398c" />

