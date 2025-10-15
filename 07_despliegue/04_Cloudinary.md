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
<summary>Otras variables (NO SE HAN UTILIZADO)</summary>
<pre>
CLOUDINARY_CLOUD_NAME=dpxxxxxxxvjfn
CLOUDINARY_KEY=46xxxxxxxxxx7173
CLOUDINARY_SECRET=bO5dxxxxxxxxxxxx_v4UcSKo
</pre>
</details>

## 6. Agregue la función para subir archivos

### ProductoController.php

```php
<?php

namespace App\Http\Controllers;

// ✂️ código omitido
use Illuminate\Support\Facades\Storage; // ➕LÍNEA AGREGADA
class ProductoController extends Controller
{
    // ✂️ código omitido
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
}
```
📚 **Notas**  
- `if (config('filesystems.default') === 'cloudinary')` retorna **true** si la variable `FILESYSTEM_DISK` tiene el valor **cloudinary** en el archivo `.env`
- `Storage::disk('cloudinary')->putFileAs('images/productos/', $img, $filename);` guarda el archivo en `Cloudinary`
- Por el momento, el archivo no se guarda en `images/productos/` sino en `Home` de `Cloudinary` (⚠️ PENDIENTE DE REVISAR).
- `$img->move(public_path("images/productos"), $filename);` guarda el archivo en la carpeta `public/images/productos/` en la carpeta del proyecto local.
- Esta es la ruta **API** definida en **api.php** `Route::post('/dashboard/productos/upload', [ProductoController::class, 'upload']);` 

## 7. Pruebe la aplicación

Ejecutar la aplicación localmente.  

```
php artisan serve
npm run dev
```

⚠️**Alerta** No se pueden subir archivos a cloudinary.  

<img width="1915" height="988" alt="imagen" src="https://github.com/user-attachments/assets/e6e0627b-40ab-4aec-bedc-85677277c8e2" />

**Más información del error**  
<img width="1919" height="967" alt="imagen" src="https://github.com/user-attachments/assets/e2855bc4-7209-49ce-ba3b-f9cbcc9843bd" />

ℹ️**Información** después de varias pruebas se determinó que el problema es por una política del navegador web **strict-origin-when-cross-origin** que no permite la acción solicitada por cuestiones de seguridad. Posteriormente creé una nueva imagen de **docker** y desplegué la aplicación en **koyeb.com** y de esta manera sí funcionó. Para ejecutar la aplicación de forma local creo que debería realizar otras configuraciones (⚠️ pendiente).  
