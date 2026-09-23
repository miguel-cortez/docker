# Configurar base de datos

## Paso 1. Clic en Connect

<img width="920" height="430" alt="imagen" src="https://github.com/user-attachments/assets/be206195-17f3-44de-9eef-f0f460b00ab8" />

## Paso 2. Generar Password

<img width="763" height="477" alt="imagen" src="https://github.com/user-attachments/assets/8c8729de-28d4-4120-b4da-2554019b5ed6" />  

### 🔑 Información para establecer conexión con la base de datos  

<img width="715" height="589" alt="imagen" src="https://github.com/user-attachments/assets/c66a5410-52d1-4912-be5b-a1a6d7f927b9" />


## Paso 3. Codificar el certificado en base64

## CA cert descargado

<img width="145" height="39" alt="imagen" src="https://github.com/user-attachments/assets/64802d5f-9b49-4544-bb8d-2e25e970d50f" />  

```
-----BEGIN CERTIFICATE-----
MIIFazCCA1OgAwIBAgIRAIIQz7DSQONZRGPgu2OCiwAwDQYJKoZIhvcNAQELBQAw
TzELMAkGA1UEBhMCVVMxKTAnBgNVBAoTIEludGVybmV0IFNlY3VyaXR5IFJlc2Vh
🎈 LÍNEAS BORRADAS
mRGunUHBcnWEvgJBQl9nJEiU0Zsnvgc/ubhPgXRR4Xq37Z0j4r7g1SgEEzwxA57d
emyPxgcYxn/eR44/KJ4EBs+lVDR3veyJm+kXQ99b21/+jh5Xos1AnX5iItreGCc=
-----END CERTIFICATE-----
```

### Certificado convertido a BASE64

<img width="115" height="43" alt="imagen" src="https://github.com/user-attachments/assets/cc78994f-c5b4-4aa5-96ce-7bc956efa2c0" />  

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1 🎈 CARACTERES BORRADOS ZJQ0FURS0tLS0tCg==
```

### Comando para convertir el certificado a BASE64

📚 ***Notas***
- El comando `base64` es nativo de Linux.
- Yo ejecuté el comando en **Windows** desde la carpeta **Downloads** en la **Terminal**
- :warning: El **miércoles 23 de septiembre de 2026** no me funcionó tal como lo describo en la línea anterior. Investigando, encontré que el comando `base64` nunca ha sido incluído en `Windows 10` ni `Windows 11`. Entonces, es probable que algún software de terceros tenía la herramienta `base64`. He comprobado que en la consola de `GitBash` sí está disponible el comando `base64`. Además, recuerdo que tenía instalado el entorno `Cygwin` que permite instalar paquetes y ejecutar comandos de Linux. En conclusión, por algún software que no es nativo de Windows puede ejecutar el comando `base64` en Windows.  
- Durante las prácticas (en capacitación), el archivo **isrgrootx1.pem** lo copiamos en la carpeta raiz del proyecto de Laravel y luego desde la distribución de Ubuntu que instalamos en WSL2 ingresamos a la carpeta raíz y ejecutamos el comando; pero pienso que no es necesario porque al final, lo que se busca es disponer del certificado en BASE64 que es un código en una sola línea y relativamente más corto que el contenido original. 


El comando que debemos ejecutar es:  
```bash
base64 -w 0 isrgrootx1.pem > ca.pem.b64
```

<img width="653" height="135" alt="imagen" src="https://github.com/user-attachments/assets/22865f09-8c8c-4dd6-af5a-88c50d3f7264" />

***Para ejecutar el comando desde Ubuntu***

```bash
cd /mnt/c
cd Users
cd Miguel
cd Download
base64 -w 0 isrgrootx1.pem > ca.pem.b64
```




