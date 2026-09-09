# Configurar TiDB Cloud en el proyecto  

## Paso 1. Modifique el archivo config/database.php  

Localice el código:  

```PHP
'options' => extension_loaded('pdo_mysql') ? array_filter([
    PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'), // 💊 ESTA ES LA LÍNEA QUE SE MODIFICÓ  
]) : [],
```

y modifíquelo como se muestra a continuación:  

```php
<?php
    // ✂ Código omitido
    'connections' => [
        // ✂ Código omitido
        'mysql' => [
            // ✂ Código omitido
            'strict' => true,
            'engine' => 'InnoDB',
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                    // 💡 SE MODIFICÓ DESDE AQUÍ  
                    PDO::MYSQL_ATTR_SSL_CA => (function () {
                    $b64 = env('MYSQL_ATTR_SSL_CA_B64');
                    if (!$b64) return null;

                    // descodificar
                    $caContent = base64_decode($b64);

                    // Guardar en archivo temporal
                    $tmpFile = sys_get_temp_dir() . '/ca_' . uniqid() . '.pem';
                    file_put_contents($tmpFile, $caContent);

                    return $tmpFile;
                })(),
                // 💡 HASTA AQUÍ
            ]) : [],
        // ✂ Código omitido
];
```
## Paso 2. En el archivo .env cambie las credenciales para conectarse a la base de datos

```php
DB_CONNECTION=mysql
DB_HOST=gateway01...tidbcloud.com
DB_PORT=4000
DB_DATABASE=test
DB_USERNAME=3XJ...root
DB_PASSWORD=dQ..TC1O
```
⚠️ Debe usar sus credenciales (obtenidas de TiDB Cloud).  

## Paso 3. Agregue al archivo .env la variable MYSQL_ATTR_SSL_CA_B64 

⚡ Es la clave que generó con el comando `base64 -w 0 isrgrootx1.pem > ca.pem.b64`  

```php
MYSQL_ATTR_SSL_CA_B64=LS0tLS1CRUdJTiBDRVJUSU ... Q0FURS0tLS0tCg==
```
La ***clave*** es mucho más grande que el ejemplo mostrado arriba.  

## Paso 4. Ejecute las migraciones  

<img width="1231" height="324" alt="imagen" src="https://github.com/user-attachments/assets/9f1f74e5-c38d-4582-91da-0fab417030b3" />

***Listo*** ya puede utilizar la base de datos **test** que creo en **TiDB Cloud**  

💎Durante la capacitación, nosotros creamos las tablas de base de datos con este comando:  

```bash
mysql --comments --connect-timeout 150 -u '<your_username>' -h <your_cluster_host> -P 4000 -D test --ssl-mode=VERIFY_IDENTITY --ssl-ca=<your_ca_path> -p<your_password> < product_data.sql
```

Ejemplo completo:  
```
mysql --comments --connect-timeout 150 -u '2puvRTdtCXSZp6Z.root' -h gateway01.us-east-1.prod.aws.tidbcloud.com -P 4000 -D test --ssl-mode=VERIFY_IDENTITY --ssl-ca=isrgrootx1.pem -pss8rqUIX4nUONld7 <  product_data.sql
``` 



