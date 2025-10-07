# Publicar imagen en Docker Hub

## Autenticación desde la terminal de Ubuntu  

<img width="898" height="250" alt="imagen" src="https://github.com/user-attachments/assets/4be91d41-1cee-4061-9aec-31f6377f8793" />

## Crear una nueva etiqueta para la imgen existente
Se necesita craer una nueva imagen para que sea compatible con el formato requerido por **Docker Hub**  
```bash
docker tag example-app:v1.0 miguelcortez01/example-app:v1.0
```

<img width="815" height="23" alt="imagen" src="https://github.com/user-attachments/assets/7b1f0bb7-6e0d-4e2f-a6b4-ceb1aa1f9286" />

## Publicar la imagen en Docker Hub  

```bash
docker push miguelcortez01/example-app:v1.0
```  

***Publicación en proceso***  

<img width="691" height="210" alt="imagen" src="https://github.com/user-attachments/assets/3570a4bc-5ea9-482e-a8d4-5690a9bb0a6f" />

***Publicación finalizada***  
<img width="983" height="437" alt="imagen" src="https://github.com/user-attachments/assets/09594e6d-e6b1-48c7-833e-24349e28e13c" />

***Docker Hub***  

<img width="1781" height="586" alt="imagen" src="https://github.com/user-attachments/assets/1fd9ed15-c5d0-4aa2-8f4b-9cc05a61b1d2" />
