# USAR CLOUDINARY DESDE LOCALHOST

ℹ️ Durante la práctica de `Cloudinary` se realizó un proceso para subir archivos a Cloudinary y la aplicación funcionó cuando de publicó en `koyeb.com` (con imagen de docker); pero si la aplicación se ejecutaba de forma local con el comando `php artisan serve` no permitía subir archivos a `Cloudinary`. Luego de varias pruebas se logró solucionar esta situación.

## CAPTURAS DE PANTALLA

### Archivos seleccionado
><img width="1895" height="937" alt="imagen" src="https://github.com/user-attachments/assets/55c953b2-b0a1-4a03-b3a5-c49a55a5f653" />

### Después de hacer clic en Upload

Solo desaparece la imagen seleccionada pero no se agrega la información en la tabla ni se sube la imagen.

><img width="1870" height="819" alt="imagen" src="https://github.com/user-attachments/assets/65d41425-e64b-49ec-ab2b-6ed3b37fcd69" />

## HISTORIAL DE ACCIONES REALIZADAS

## Descripción del error

Al presionar `SHIFT+CTRL+I` en el navegado (Mozilla Firefox) puedo ver detalles del error.  

><img width="1919" height="365" alt="imagen" src="https://github.com/user-attachments/assets/80c6f6b2-d1ec-4257-b149-618f0df37fc2" />

><img width="1724" height="873" alt="imagen" src="https://github.com/user-attachments/assets/8437d72a-f7ff-4f33-9b82-fc8b699f9672" />


⭐ Se determinó que el problema tiene que ver con la política del navegador web **strict-origin-when-cross-origin** que restringe peticiones de dominios diferentes. Posteriormente creé una imagen de **docker** y desplegué la aplicación en **koyeb.com**. El resultado fue satisfactorio pero con la aplicación ya desplegada (en producción). Hasta este punto, localmente sigue sin funcionar.


## Creación y uso de un Middleware

### Middleware

```
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;
// Este Middleware ha sido creado por MACV (no venía por defecto)
class HanddleCors
{
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        $response = $next($request);

        $origin = $request->headers->get('Origin');

        $allowedOrigins = [
            'http://localhost:8000',
            'URL DE CLOUDINARY',
        ];

        if (in_array($origin, $allowedOrigins)) {
            $response->headers->set('Access-Control-Allow-Origin', $origin);
            $response->headers->set('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
            $response->headers->set('Access-Control-Allow-Headers', 'Content-Type, Authorization');
            $response->headers->set('Access-Control-Allow-Credentials', 'true');
        }

        return $response;
    }
}
```
### Se registró el Middleware en bootstrap/app.php

```
<?php

// código omitido
use App\Http\Middleware\HanddleCors;
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    // código omitido
    // SE AGREGÓ DESDE AQUÍ
    ->withMiddleware(function (Middleware $middleware) {

        $middleware->append(HanddleCors::class);

    })
    // HASTA AQUÍ
    ->create();
```

### Uso del Middleware en la ruta API

```
Route::post('/dashboard/productos/upload', [ProductoController::class, 'upload'])->middleware([HanddleCors::class]);
```

⭐ El error siempre continúa.  


## Nuevo error presentado en las opciones de desarrollo

Al parecer no soporta el driver `cloudinary`  

><img width="686" height="218" alt="imagen" src="https://github.com/user-attachments/assets/4b068961-1fa9-4867-ac20-6883c50f7997" />

## Realicé una prueba en tinker

```php
php artisan tinker
Storage::disk('cloudinary')->put('test.txt', 'test content');
```
><img width="526" height="144" alt="imagen" src="https://github.com/user-attachments/assets/34d08471-8cd4-4d46-8370-5764ff3199b1" />

⭐El mensaje parece ser el mismo. Está relacionado con el driver de `cloudinary` 

## Actualicé paquetes de composer

```
composer update
```

## Realicé nuevas pruebas en tinker

```php
Storage::disk('cloudinary')->put('test.txt', 'test content');
```
Nuevo error mostrado en tinker:  

<img width="622" height="169" alt="imagen" src="https://github.com/user-attachments/assets/b29882f7-43dc-4185-84cb-c444bb32858c" />

Al parecer no es correcta la sintaxis usada en el método **put()** porque intenta abrir un archivo llamado `test content` que no existe.  


Creé un archivo llamdo **test.txt** y ejecuté el siguiente comando en tinker `Storage::disk('cloudinary')->putFile('C:\wamp64\www\example-app\test.txt');` 

Ahora sí aceptó la sintaxis; pero generó otro error:  

><img width="1438" height="181" alt="imagen" src="https://github.com/user-attachments/assets/d808383d-2c71-4f0d-b735-3425beb71f43" />


Ahora el mensaje ha cambiado por `cURL error 60: SSL certificate problem: unable to get local issuer certificate (see https://curl.haxx.se/libcurl/c/libcurl-errors.html) for https://api.cloudinary.com/v1_1/dpj56vjfn/raw/upload` (visto en la pestaña Respuesta).    

## Realicé nuevas pruebas en el navegador web (opciones de desarrollo) 

><img width="1043" height="199" alt="imagen" src="https://github.com/user-attachments/assets/fd88454c-0fda-46a9-b35d-a2057d2c16fb" />

También el mensaje de error ha cambiado por `cURL error 60: SSL certificate problem: unable to get local issuer certificate (see https://curl.haxx.se/libcurl/c/libcurl-errors.html) for https://api.cloudinary.com/v1_1/dpj56vjfn/raw/upload` (visto en la pestaña Respuesta).    

## FINALMENTE ... LA SOLUCIÓN
🔑 Se debe descargar el certificado **cacert.pem** desde **https://curl.se/ca/cacert.pem** y configurarlo en **php.ini** de la versión de PHP que está utilizando el proyecto de Laravel. Yo guardé el archivo **cacert.pem** en el directorio **c:\cacert**  

<details>
    <summary>Contenido parcial de cacert.pem</summary>
    <pre>
    ##
    ## Bundle of CA Root Certificates
    ##
    ## Certificate data from Mozilla as of: Tue Sep  9 03:12:01 2025 GMT
    ##
    ## Find updated versions here: https://curl.se/docs/caextract.html
    ##
    ## This is a bundle of X.509 certificates of public Certificate Authorities
    ## (CA). These were automatically extracted from Mozilla's root certificates
    ## file (certdata.txt).  This file can be found in the mozilla source tree:
    ## https://raw.githubusercontent.com/mozilla-firefox/firefox/refs/heads/release/security/nss/lib/ckfw/builtins/certdata.txt
    ##
    ## It contains the certificates in PEM format and therefore
    ## can be directly used with curl / libcurl / php_curl, or with
    ## an Apache+mod_ssl webserver for SSL client authentication.
    ## Just configure this file as the SSLCACertificateFile.
    ##
    ## Conversion done with mk-ca-bundle.pl version 1.29.
    ## SHA256: 0078e6bdd280fd89e1b883174387aae84b3eae2ee263416a5f8a14ee7f179ae9
    ##
    
    
    Entrust Root Certification Authority
    ====================================
    -----BEGIN CERTIFICATE-----
    MIIEkTCCA3mgAwIBAgIERWtQVDANBgkqhkiG9w0BAQUFADCBsDELMAkGA1UEBhMCVVMxFjAUBgNV
    BAoTDUVudHJ1c3QsIEluYy4xOTA3BgNVBAsTMHd3dy5lbnRydXN0Lm5ldC9DUFMgaXMgaW5jb3Jw
    b3JhdGVkIGJ5IHJlZmVyZW5jZTEfMB0GA1UECxMWKGMpIDIwMDYgRW50cnVzdCwgSW5jLjEtMCsG
    A1UEAxMkRW50cnVzdCBSb290IENlcnRpZmljYXRpb24gQXV0aG9yaXR5MB4XDTA2MTEyNzIwMjM0
    MloXDTI2MTEyNzIwNTM0MlowgbAxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1FbnRydXN0LCBJbmMu
    MTkwNwYDVQQLEzB3d3cuZW50cnVzdC5uZXQvQ1BTIGlzIGluY29ycG9yYXRlZCBieSByZWZlcmVu
    Y2UxHzAdBgNVBAsTFihjKSAyMDA2IEVudHJ1c3QsIEluYy4xLTArBgNVBAMTJEVudHJ1c3QgUm9v
    dCBDZXJ0aWZpY2F0aW9uIEF1dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
    </pre>
</details>


***Contenido parcial de php.ini antes de la modificación***  
><img width="1292" height="726" alt="imagen" src="https://github.com/user-attachments/assets/b4530dd2-d3ca-48a0-bb9a-dfbc1e2e13db" />

***Contenido parcial de php.ini después de la modificación***  
><img width="1259" height="557" alt="imagen" src="https://github.com/user-attachments/assets/cdea00dc-ff68-4b58-892a-9326a1ac1050" />

📚 Estas son las líneas habilitadas:  

```
curl.cainfo = "C:\cacert\cacert.pem"
```

```
openssl.cafile = "C:\cacert\cacert.pem"
```

Se reinició el servidor con `php artisan serve` y listo. Ahora sí funciona.  

<img width="1852" height="667" alt="imagen" src="https://github.com/user-attachments/assets/c266980b-b00d-4708-b0ec-61fd8e163467" />

🔤 Comentario adicional

Tabién localicé otro archivo `cacert.pem` en la ubicación `C:\wamp64\apps\phpmyadmin5.2.1\vendor\composer\ca-bundle\res\cacert.pem`. Probé con este certificado y también funcionó (pero revisé las fechas y este certificado es más antiguo). Dicho de otra forma, descargar el certificado no era necesario pues lo que faltaba era habilitarlo en **php.ini**  

><img width="1437" height="485" alt="imagen" src="https://github.com/user-attachments/assets/ac805c85-903f-47f7-9388-e7dd484e6a4e" />



***Fecha del ceritificado que ya estaba en el equipo***  
><img width="1622" height="458" alt="imagen" src="https://github.com/user-attachments/assets/6e2fb0a5-432d-482b-9f33-348bbed35e63" />

***Fecha del ceritificado que descargado***  
><img width="1833" height="516" alt="imagen" src="https://github.com/user-attachments/assets/9273a884-83e1-4558-b4dd-91eb852a34bd" />
