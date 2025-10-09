# Desplegar una aplicación en Koyeb.com

## Paso 1. Inicie sesión en Koyeb.com

## Paso 2. Cree un nuevo servicio

<img width="980" height="342" alt="imagen" src="https://github.com/user-attachments/assets/34884762-a173-4516-81d7-e75965d70ecf" />


### Importe el proyecto

En la parte derecha de **Image** debe escribir el nombre de la imagen que quiere importar desde **Docker Hub**. En el ejemplo se está importando la imagen **miguelcortez01/example_app:v1.0**. Si la imagen está disponible se mostrará el mensaje ***The image is accessible***  

<img width="980" height="492" alt="imagen" src="https://github.com/user-attachments/assets/7d493778-a9d3-4ac7-bdac-256f2d96948c" />

## Seleccione la instancia y región

<img width="935" height="535" alt="imagen" src="https://github.com/user-attachments/assets/45b66c8c-0e2c-429f-9fe3-bd27f7731364" />


## Configure las variables de entorno y haga clic en Deploy 

<img width="980" height="485" alt="imagen" src="https://github.com/user-attachments/assets/49d2045c-b66b-43e2-b3a3-cdf587eaef21" />

### Copie todo el contenido del archivo .env y pegue en el cuadro de diálogo siguiente:  
<img width="849" height="506" alt="imagen" src="https://github.com/user-attachments/assets/5020b2c9-c0f5-454d-8409-783b47158045" />


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

