# Usar Cloudiny desde localhost

ℹ️ Durante la práctica de Cloudinary se realizó un proceso para subir archivos a Cloudinary y la aplicación funcionó cuando de publicó el sitio web en koyeb.com; pero si la aplicación se ejecuta de forma local con el comando `php artisan serve` no permite subir archivos a Cloudinary. Luego de varias pruebas se logró solucionar esta situación. En este documento se presenta un historial de las acciones realizadas; pero si quiere ir directo a la solución, vaya al final de este documento.

## Error mostrado

><img width="1915" height="988" alt="imagen" src="https://github.com/user-attachments/assets/e6e0627b-40ab-4aec-bedc-85677277c8e2" />

## Más información del error

><img width="1919" height="967" alt="imagen" src="https://github.com/user-attachments/assets/e2855bc4-7209-49ce-ba3b-f9cbcc9843bd" />

ℹ️**Información** después de varias pruebas se determinó que el problema es por una política del navegador web **strict-origin-when-cross-origin** que no permite la acción solicitada por cuestiones de seguridad. Posteriormente creé una nueva imagen de **docker** y desplegué la aplicación en **koyeb.com** y de esta manera sí funcionó. Para ejecutar la aplicación de forma local creo que debería realizar otras configuraciones.  

## Otras pruebas

><img width="686" height="218" alt="imagen" src="https://github.com/user-attachments/assets/4b068961-1fa9-4867-ac20-6883c50f7997" />

```php
Storage::disk('cloudinary')->put('test.txt', 'test content');
```
><img width="526" height="144" alt="imagen" src="https://github.com/user-attachments/assets/34d08471-8cd4-4d46-8370-5764ff3199b1" />

><img width="1043" height="199" alt="imagen" src="https://github.com/user-attachments/assets/fd88454c-0fda-46a9-b35d-a2057d2c16fb" />
>

```
"cURL error 60: SSL certificate problem: unable to get local issuer certificate (see https://curl.haxx.se/libcurl/c/libcurl-errors.html) for https://api.cloudinary.com/v1_1/dpj56vjfn/image/upload"
```

<img width="1438" height="181" alt="imagen" src="https://github.com/user-attachments/assets/d808383d-2c71-4f0d-b735-3425beb71f43" />

<img width="1292" height="726" alt="imagen" src="https://github.com/user-attachments/assets/b4530dd2-d3ca-48a0-bb9a-dfbc1e2e13db" />

<img width="1259" height="557" alt="imagen" src="https://github.com/user-attachments/assets/cdea00dc-ff68-4b58-892a-9326a1ac1050" />


<img width="1852" height="667" alt="imagen" src="https://github.com/user-attachments/assets/c266980b-b00d-4708-b0ec-61fd8e163467" />
