# Configurar base de datos

## Paso 1. Clic en Connect

<img width="920" height="430" alt="imagen" src="https://github.com/user-attachments/assets/be206195-17f3-44de-9eef-f0f460b00ab8" />

## Paso 2. Generar clave

<img width="763" height="477" alt="imagen" src="https://github.com/user-attachments/assets/8c8729de-28d4-4120-b4da-2554019b5ed6" />  

**Credenciales generadas**  

<img width="684" height="495" alt="imagen" src="https://github.com/user-attachments/assets/3e0ef896-1c19-420a-ba72-2e821bdcbc13" />

## Paso 3. Codificar el certificado en base64
**Este es el certificado descargado**  
<img width="145" height="39" alt="imagen" src="https://github.com/user-attachments/assets/64802d5f-9b49-4544-bb8d-2e25e970d50f" />

**Comando para convertirlo a base64**  

```
base64 -w 0 isrgrootx1.pem > ca.pem.b64
```

**Este es el archivo creado en proceso de conversión**  

<img width="115" height="43" alt="imagen" src="https://github.com/user-attachments/assets/cc78994f-c5b4-4aa5-96ce-7bc956efa2c0" />


ℹ️ Yo ejecuté el comando en Windows; pero también se puede ejecutar desde Ubuntu. Durante las prácticas (en capacitación) este comando lo ejecutamos dentro de la carpeta del proyecto y desde la consola de Ubuntu; pero realmente creo que no es necesario porque lo único que necesitamos es reducir el tamaño del certificado original (CA cert) en una cadena más pequeña pero equivalente (base64).  Esta cadena equivalente es la que utilizamos luego en el archivo **.env**  

<img width="653" height="135" alt="imagen" src="https://github.com/user-attachments/assets/22865f09-8c8c-4dd6-af5a-88c50d3f7264" />



