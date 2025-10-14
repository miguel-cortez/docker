# Cloudinary

## Crear cuenta

Ingrese al **https://cloudinary.com/** y cree una cuenta haciendo clic en **GET STARTER** y luego en **SIGN UP WITH EMAIL**  

<img width="1202" height="730" alt="imagen" src="https://github.com/user-attachments/assets/935d1234-9b83-42a1-a682-98e1439deb1e" />


<img width="647" height="588" alt="imagen" src="https://github.com/user-attachments/assets/942ea829-3a34-476a-801e-b857d2da19f2" />


## Instalación de paquete

```bash
composer require cloudinary-labs/cloudinary-laravel
```

<img width="1104" height="612" alt="500677302-06a5305b-721f-4021-9ba6-359c820f8c1b" src="https://github.com/user-attachments/assets/4ea2cc27-118f-4dd8-a55e-413f7ec6eb1c" />


```bash
php artisan cloudinary:install
```
<img width="841" height="487" alt="500677523-ea688b39-4b91-426b-99a3-ae2216e8f8f7" src="https://github.com/user-attachments/assets/3fc56934-4615-453c-8563-7f28463d81b0" />



## Modifique el archivo config/filesystems.php

### config/filesystems.php (ORIGINAL)

```php
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
```


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

## Copie CLOUDINARY_URL

<img width="740" height="208" alt="imagen" src="https://github.com/user-attachments/assets/9f0db08e-c17f-4701-85a1-ac06ca493bad" />

<img width="740" height="241" alt="imagen" src="https://github.com/user-attachments/assets/69a80559-5cd0-40a6-83eb-3cbf40be73b6" />

<img width="716" height="355" alt="imagen" src="https://github.com/user-attachments/assets/e907eddf-e75d-4319-9f1e-1580b1c8f16b" />

<img width="719" height="364" alt="imagen" src="https://github.com/user-attachments/assets/739d4abb-21d0-4842-97e8-9b900c0d7104" />


## Modifique el archivo .env

```
APP_NAME=Laravel
APP_ENV=local
// ✂️CÓDIGO OMITIDO
#FILESYSTEM_DISK=local
FILESYSTEM_DISK=cloudinary  // 👈EQUIVALE A LA LÍNEA ANTERIOR PERO PARA USAR CLOUDINARY, NO ALMACENAMIENTO LOCAL.
// ✂️ CÓDIGO OMITIDO

# Cloudinary
CLOUDINARY_URL=cloudinary://467478xxxxxxxxxxxxxxxxxxxx4UcSKo@dpj56vjfn // 👈ESTA LÍEA LA OBTIENE DE CLOUDINARY.COM (DEL DASHBOARD).

```
