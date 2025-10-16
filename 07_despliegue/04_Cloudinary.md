# Cloudinary

## 1. Crear cuenta

Ingrese al **https://cloudinary.com/** y cree una cuenta haciendo clic en **GET STARTER** y luego en **SIGN UP WITH EMAIL**  

><img width="1202" height="730" alt="imagen" src="https://github.com/user-attachments/assets/935d1234-9b83-42a1-a682-98e1439deb1e" />


<img width="647" height="588" alt="imagen" src="https://github.com/user-attachments/assets/942ea829-3a34-476a-801e-b857d2da19f2" />


## 2. Instalación de paquete

```bash
composer require cloudinary-labs/cloudinary-laravel
```

<img width="1104" height="612" alt="500677302-06a5305b-721f-4021-9ba6-359c820f8c1b" src="https://github.com/user-attachments/assets/4ea2cc27-118f-4dd8-a55e-413f7ec6eb1c" />


```bash
php artisan cloudinary:install
```
<img width="841" height="487" alt="500677523-ea688b39-4b91-426b-99a3-ae2216e8f8f7" src="https://github.com/user-attachments/assets/3fc56934-4615-453c-8563-7f28463d81b0" />



## 3. Modifique el archivo config/filesystems.php

<details close>
<summary>config/filesystems.php (ORIGINAL)</summary>
<pre>
<?php

return [

    /*
    |--------------------------------------------------------------------------
    | Default Filesystem Disk
    |--------------------------------------------------------------------------
    |
    | Here you may specify the default filesystem disk that should be used
    | by the framework. The "local" disk, as well as a variety of cloud
    | based disks are available to your application for file storage.
    |
    */

    'default' => env('FILESYSTEM_DISK', 'local'),

    /*
    |--------------------------------------------------------------------------
    | Filesystem Disks
    |--------------------------------------------------------------------------
    |
    | Below you may configure as many filesystem disks as necessary, and you
    | may even configure multiple disks for the same driver. Examples for
    | most supported storage drivers are configured here for reference.
    |
    | Supported drivers: "local", "ftp", "sftp", "s3"
    |
    */

    'disks' => [

        'local' => [
            'driver' => 'local',
            'root' => storage_path('app/private'),
            'serve' => true,
            'throw' => false,
            'report' => false,
        ],

        'public' => [
            'driver' => 'local',
            'root' => storage_path('app/public'),
            'url' => env('APP_URL').'/storage',
            'visibility' => 'public',
            'throw' => false,
            'report' => false,
        ],

        's3' => [
            'driver' => 's3',
            'key' => env('AWS_ACCESS_KEY_ID'),
            'secret' => env('AWS_SECRET_ACCESS_KEY'),
            'region' => env('AWS_DEFAULT_REGION'),
            'bucket' => env('AWS_BUCKET'),
            'url' => env('AWS_URL'),
            'endpoint' => env('AWS_ENDPOINT'),
            'use_path_style_endpoint' => env('AWS_USE_PATH_STYLE_ENDPOINT', false),
            'throw' => false,
            'report' => false,
        ],

    ],

    /*
    |--------------------------------------------------------------------------
    | Symbolic Links
    |--------------------------------------------------------------------------
    |
    | Here you may configure the symbolic links that will be created when the
    | `storage:link` Artisan command is executed. The array keys should be
    | the locations of the links and the values should be their targets.
    |
    */

    'links' => [
        public_path('storage') => storage_path('app/public'),
    ],

];
</pre>
</details>



### config/filesystems.php (MODIFICADO)
```php

<?php

return [

    // ✂️ CÓDIGO OMITIDO

    'disks' => [

        // ✂️CÓDIGO OMITIDO

        's3' => [
            'driver' => 's3',
            'key' => env('AWS_ACCESS_KEY_ID'),
            'secret' => env('AWS_SECRET_ACCESS_KEY'),
            'region' => env('AWS_DEFAULT_REGION'),
            'bucket' => env('AWS_BUCKET'),
            'url' => env('AWS_URL'),
            'endpoint' => env('AWS_ENDPOINT'),
            'use_path_style_endpoint' => env('AWS_USE_PATH_STYLE_ENDPOINT', false),
            'throw' => false,
            'report' => false,
        ],
        // 💡 CODIGO AGREGADO PARA CLOUDINARY
        'cloudinary' => [
            'driver' => 'cloudinary',
            'key' => env('CLOUDINARY_KEY'),
            'secret' => env('CLOUDINARY_SECRET'),
            'cloud' => env('CLOUDINARY_CLOUD_NAME'),
            'url' => env('CLOUDINARY_URL'),
            'secure' => (bool) env('CLOUDINARY_SECURE', true),
            'prefix' => env('CLOUDINARY_PREFIX'),
        ],
        // 💡FIN DE CODIGO AGREGADO PARA CLOUDINARY
    ],
    // ✂️ CÓDIGO OMITIDO

];
```

## 4. Obtenga información de las credenciales

📚 **Nota** Puede obtener los valores de las siguientes variables: CLOUDINARY_URL, CLOUDINARY_CLOUD_NAME, CLOUDINARY_KEY, CLOUDINARY_SECRET pero en este ejemplo solo se está utilizando **CLOUDINARY_URL**.

><img width="740" height="208" alt="imagen" src="https://github.com/user-attachments/assets/9f0db08e-c17f-4701-85a1-ac06ca493bad" />  

><img width="740" height="241" alt="imagen" src="https://github.com/user-attachments/assets/69a80559-5cd0-40a6-83eb-3cbf40be73b6" />  

><img width="716" height="355" alt="imagen" src="https://github.com/user-attachments/assets/e907eddf-e75d-4319-9f1e-1580b1c8f16b" />  

><img width="719" height="364" alt="imagen" src="https://github.com/user-attachments/assets/739d4abb-21d0-4842-97e8-9b900c0d7104" />  


## 5. Modifique el archivo .env

```
APP_NAME=Laravel
APP_ENV=local
// ✂️CÓDIGO OMITIDO
FILESYSTEM_DISK=cloudinary  // 👈LINEA MODIFICADA. El valor original era local
// ✂️ CÓDIGO OMITIDO

# Cloudinary
CLOUDINARY_URL=cloudinary://467478xxxxxxxxxxxxxxxxxxxx4UcSKo@dpj56vjfn // 👈LÍNEA AGREGADA. SE OBTIENE DE CLOUDINARY (PASO ANTERIOR)

```

<details close>
<summary>Otras variables</summary>
<pre>
CLOUDINARY_CLOUD_NAME=dpxxxxxxxvjfn
CLOUDINARY_KEY=46xxxxxxxxxx7173
CLOUDINARY_SECRET=bO5dxxxxxxxxxxxx_v4UcSKo
</pre>
**Nota** No se han utilizado para subir las imágenes.
</details>

## 6. Agregue la función upload() en ProductoController

### ProductoController.php

```php
<?php

namespace App\Http\Controllers;

// ✂️ código omitido
use Illuminate\Support\Facades\Storage; // ➕LÍNEA AGREGADA
class ProductoController extends Controller
{
    // ✂️ código omitido
    // 👉 AGREGAR DESDE AQUÍ
    public function upload(Request $request){
        try{
            $info = array();
            //foreach ($request["image"] as $img){
            if($request->hasFile("image")){
                foreach ($request["image"] as $img){
                    $filename = Carbon::now()->timestamp . '_' . rand(1000, 9999) . '.' . $img->extension();
                    if (config('filesystems.default') === 'cloudinary') {
                        Storage::disk('cloudinary')->putFileAs('images/productos/', $img, $filename);
                    } else {
                        $img->move(public_path("images/productos"), $filename);
                    }
                    $producto = Producto::find($request["producto_id"]);
                    $producto->imagen = $filename;
                    $producto->save();
                    $info[] = $filename;
                }
                return response()->json(["data"=> $info, "message"=>"La imagen se ha guardado"],200);
            }else{
                return response()->json(["data"=> "No se han recibido archivos", "message"=>"Datos incompletos"],500);
            }
        }catch(\Exception $e){
            return response()->json(["data"=> null, "message"=>$e->getMessage()],422);
        } 
    }
// 👈HASTA AQUÍ
}
```
📚 **Notas**  
- `if (config('filesystems.default') === 'cloudinary')` retorna **true** si la variable `FILESYSTEM_DISK` tiene el valor **cloudinary** en el archivo `.env`
- `Storage::disk('cloudinary')->putFileAs('images/productos/', $img, $filename);` guarda el archivo en `Cloudinary`
- En cloudinary, la imagen se guarda en una ruta como `https://res.cloudinary.com/<cloud_name>/image/upload/v1760517019/images/productos/1760606963_3871.png`.
- `$img->move(public_path("images/productos"), $filename);` guarda el archivo en la carpeta `public/images/productos/` en la carpeta del proyecto local.
- Esta es la ruta **API** definida en **api.php** `Route::post('/dashboard/productos/upload', [ProductoController::class, 'upload']);`

## 7. Agregue una función remove() en ProductoController

### ProductoController.php

```php
<?php

namespace App\Http\Controllers;
// ✂️ CÓDIGO OMITIDO
use Illuminate\Support\Facades\Storage;
class ProductoController extends Controller
{
    // ✂️ CÓDIGO OMITIDO
    public function upload(Request $request){
        // ✂️CÓDIGO OMITIDO
    }
    // 👉 AGREGAR DESDE AQUÍ
    public function remove(Request $request){
        try{
            $producto = Producto::find($request["id"]);
            if($producto){
                $filename = $producto->imagen;
                $producto->imagen = null;
                $producto->save();
                if (config('filesystems.default') === 'cloudinary') {
                    $filePath = 'images/productos/' . $filename;
                    Storage::disk('cloudinary')->delete($filePath);
                    return response()->json(["data"=> $filePath, "message"=>"Imagen eliminada"],200);
                } else {
                    $filePath = public_path('images/productos/'.$filename);
                    if (File::exists($filePath)) {
                        File::delete($filePath);
                        return response()->json(["data"=> $filePath, "message"=>"Imagen eliminada"],200);
                    } else {
                        return response()->json(["data"=> $filePath, "message"=>"Imagen no encontrada"],404);
                    }
                }
            }else{
                return response()->json(["data"=> "Producto no encontrado", "message"=>"Imagen no eliminada"],404);
            }
        }catch(\Exception $e){
            return response()->json(["data"=> null, "message"=>$e->getMessage()],422);
        }
    }
    // 👈HASTA AQUÍ
}
```
<details close>
    <summary>Mensaje obtenido cuando se elimina desde cloudinary</summary>
    <pre>
    {
      "data": "images/productos/1760606963_3871.png",
      "message": "Imagen eliminada"
    }
    </pre>
    Nota. No obtiene la ruta absoluta.
</details>

## 8. Ejecute la aplicación

```php
php artisan serve
npm run dev
```

> **⭐ ALERTA** No se pueden subir archivos a `cloudinary` cuando la aplicación se ejecuta de forma local (no desplegada). Es probable que se pueda pero modificando alguna política de seguridad. Esto representaría un problema de seguridad para la aplicación y solo podría hacerlo para realizar prueba en entorno de desarrollo.  

><img width="1915" height="988" alt="imagen" src="https://github.com/user-attachments/assets/e6e0627b-40ab-4aec-bedc-85677277c8e2" />

**Más información del error**  
><img width="1919" height="967" alt="imagen" src="https://github.com/user-attachments/assets/e2855bc4-7209-49ce-ba3b-f9cbcc9843bd" />

ℹ️**Información** después de varias pruebas se determinó que el problema es por una política del navegador web **strict-origin-when-cross-origin** que no permite la acción solicitada por cuestiones de seguridad. Posteriormente creé una nueva imagen de **docker** y desplegué la aplicación en **koyeb.com** y de esta manera sí funcionó. Para ejecutar la aplicación de forma local creo que debería realizar otras configuraciones.  

##### PRUEBAS

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


## 9. Construya una imagen de Docker y publíquela en Docker Hub

### 9.1 Construir la imagen

```
docker image build -t example-app:v2.0 .
```

📚 **Nota** Recuerde que usted puede asignar un nombre de imagen y etiqueta según su conveniencia. 

><img width="479" height="230" alt="imagen" src="https://github.com/user-attachments/assets/341c1884-f95d-4ce5-ba78-8ae02e84fd9c" />

### 9.2 Asignar etiqueta compatible con Docker Hub
```
docker tag example-app:v2.0 miguelcortez01/example-app:v2.0
```
### 9.3 Subir la imagen a Docker Hub

```
docker push miguelcortez01/example-app:v2.0
```

## 10. Despliegue la nueva aplicación en Koyeb.com

### 10.1 Configure variables en Koyeb (Environment variables and files)

📚 **Nota** Al publicar el sitio web en **koyeb.com** debe modificar la variable `FILESYSTEM_DISK` con el valor `cloudinary` y además, debe agregar la variable `CLOUDINARY_URL` con el valor **CLOUDINARY_URL** obtenido de su cuenta personal.  

Variable modificada:  
```
FILESYSTEM_DISK=cloudinary
```
Variable agregada (➕ Add another):  
```
CLOUDINARY_URL=cloudinary://467478xxxxxxxxxxxxxxxxxxxx4UcSKo@dpj56vjfn
```

### 10.2 Agregue la imagen de docker que quiere desplegar (source)
><img width="777" height="617" alt="imagen" src="https://github.com/user-attachments/assets/7ce054c3-b592-4af1-9608-71bff207a590" />

### 10.3 Despliegue la aplicación
><img width="152" height="51" alt="imagen" src="https://github.com/user-attachments/assets/e82de630-09fc-42c7-91aa-a8b003ec67ef" />




