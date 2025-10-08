# Desplegar aplicación desde una imagen de Docker Hub

## Cree una cuenta en koyeb.com

[https://www.koyeb.com/](https://www.koyeb.com/)  

<img width="902" height="498" alt="imagen" src="https://github.com/user-attachments/assets/77d7fdd6-3e28-4ab2-a8ce-ed63781f1937" />

**Llene el formulario:**  

<img width="345" height="495" alt="imagen" src="https://github.com/user-attachments/assets/2db48ba5-a31e-4827-966e-b365868adc6e" />

## Desplegar una aplicación  

**Cree un nuevo servicio de Docker**  
<img width="943" height="437" alt="imagen" src="https://github.com/user-attachments/assets/0e1c1683-118f-46d6-88ae-9129ee700c89" />

**Escriba el nombre del imagen que quiere desplegar**  

<img width="913" height="402" alt="imagen" src="https://github.com/user-attachments/assets/64807f33-3f24-4c64-a7da-22c33ee67441" />  

**Seleccione el plan CPU Eco**  

<img width="703" height="682" alt="imagen" src="https://github.com/user-attachments/assets/745bbdd3-46f2-4acb-920c-cc366e734d36" />

**Configure las variables de entorno y haga clic en Deploy**  

<img width="686" height="678" alt="imagen" src="https://github.com/user-attachments/assets/04938069-95a0-4fa5-acb5-2ac5756049f1" />

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


