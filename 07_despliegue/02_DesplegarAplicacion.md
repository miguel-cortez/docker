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

<img width="980" height="470" alt="imagen" src="https://github.com/user-attachments/assets/ba4b3b8d-9533-4c96-bf9e-3cc374bef381" />

## Paso 3. Pruebe la apliación en nagevador Web. 

***Haga clic en el link que le generó de manera automática como se ve en la siguiente imagen***  

<img width="518" height="80" alt="imagen" src="https://github.com/user-attachments/assets/b359d6b2-5b19-43ee-a083-fabeb9412c91" />


🔖***NO APLICA LOS ESTILOS DE CSS***. Al ingresar a las **Herramientas para desarrolladores*** se puede ver que tampoco carga los archivos de Javascript (js).   

### Sitio web visto en Google Chrome**

<img width="1918" height="860" alt="imagen" src="https://github.com/user-attachments/assets/10a842d7-e9c9-4d2c-88c4-4a04eda6a02b" />


### Sitio web visto en Mozilla Firefox

<img width="1875" height="894" alt="imagen" src="https://github.com/user-attachments/assets/45f334b4-e00b-4f6c-b94e-173accdf0a3c" />



### Causas del error  
- Si los estilos CSS no se aplican al sitio web se puede deber a varias razones como: no se ha agregado al contenedor los archivos css y js, problemas de permisos en la carpeta public/build, etc.
- 🔑 En este caso el error se debe a que ***el sitio web publicado utiliza el protocolo https y los archivos css y js están tratando de cargar mediante el protocolo http***

## Paso 4. Solución del error de estilos y carga de JS.

### Modifique el archivo AppServiceProvider.ph

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

### Construya nuevamente la imagen de docker.

***Ejemplo:***

```
docker image build -t example_app:v2.0 .
```

### Cree una etiqueta compatible para Docker Hub
```
docker tag example_app:v2.0 miguelcortez01/example_app:v2.0
```

### Suba la imagen a Docker Hub

```bash
docker push miguelcortez01/example_app:v2.0
```

### Cambie las configuraciones de la aplicación (en Koyeb)

<img width="980" height="416" alt="imagen" src="https://github.com/user-attachments/assets/95213517-c1d7-4b6b-ad82-700904327062" />

💡Debe cambiar por ejemplo **miguelcortez01/example_app:v1.0** por **miguelcortez01/example_app:v2.0**  

### Modifique las siguientes variables de entorno 

```bash
APP_DEBUG: false
APP_ENV: production
APP_URL: https://limited-kiri-macvorganization-5368fa94.koyeb.app/
LOG_LEVEL: info
SESSION_SECURE_COOKIE=true // 🔖 ES ES VARIABLE NUEVA
```

📚 La URL `https://limited-kiri-macvorganization-5368fa94.koyeb.app/` solo es un ejemplo.

<img width="835" height="471" alt="imagen" src="https://github.com/user-attachments/assets/497d6861-e8a0-4d86-8763-9cb84a6a465e" />

### Guarde y despliegue la aplicación

<img width="950" height="297" alt="imagen" src="https://github.com/user-attachments/assets/a1623393-23b1-4ed4-9c05-43852d2cd112" />


## Paso 5. Pruebe nuevamente la aplicación en el navegador web

<img width="996" height="653" alt="imagen" src="https://github.com/user-attachments/assets/0ab16c05-a670-4e67-bb08-4fb75381398c" />

