# Workshop Container Instances

### Requerimientos:

- Cuenta de Oracle Cloud Infrastructure(test gratuito https://www.oracle.com/cloud/free/)
- Cuenta de Github (https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
- Poseer servidor linux con las siguientes dependencias instaladas:
  - git
  - python36-oci-cli
  - podman - docker 

### ¿Qué vamos a hacer?
Todo el código está en https://github.com/whiplash0104/Workshop-Function


### Paso a Paso

1. Crear Cuenta en git, usuar correo empresarial o personal (github.com)


https://user-images.githubusercontent.com/14284928/219779574-ad656998-a63d-402a-87cd-b9658c84e401.mov


2. Creación de compartment con el nombre de cada uno ej: **fbasso**
```
Menú Principal > Identity & Security > Compartments > Create Compartment
```


https://github.com/user-attachments/assets/b5a06579-a0a5-49b2-ab0c-cf1f8cd406c7

3. Creación de Registry con el nombre app1 y el nombre de usuario, ej: app1fbasso
```
Menú Principal > Developer Services > Container Registry > Create Repository
```
https://github.com/user-attachments/assets/5f22c213-958d-40af-9fd3-231a42eac5f4

```
Una vez creado el registry, copiar el namespace
```
![image](https://github.com/user-attachments/assets/35bfa005-7806-4afc-ac9a-d88bf2bc8e99)


4. Crear Token para conexión a registry
```
Esquina superior derecha
Profile > User Setting > Auth tokens > Generate Token
El token debe ser copiado y almacenado, ya que no se puede borrar
```

https://github.com/user-attachments/assets/2dc9a168-f2c9-45b7-83e9-2590944e6a41

5. Conectar por ssh a la instancia 129.159.26.160, las credenciales serán entregadas en el laboratorio

6. Creación de imágen y subirla a registry, ejecutar los siguientes comandos:
```
CREAR IMÁGEN
$ cd Docker/
$ podman build --tag NOMBRE_REGISTRY -f Dockerfile                  # En mi caso es podman build --tag app1fbasso -f Dockerfile
$ podman images
```

https://github.com/user-attachments/assets/ed6119b4-a43c-46f2-b865-d01e4f7681c4

```
SUBIR IMAGEN
$ podman login fra.ocir.io
        Usuario: NAMESPACE/USUARIO
        Pass: token creado previamente
$ podman tag localhost/app1fbasso:latest fra.ocir.io/frdpzymjc7jw/app1fbasso:latest
$ podman push fra.ocir.io/frdpzymjc7jw/app1fbasso:latest
```

https://github.com/user-attachments/assets/c4bc8823-1e63-4226-8f90-bb6dc94754ed

7. Crear VCN en compartmente utilizando el Wizard
```
Menú Principal > Networking Virtual Cloud Netowrks > Start VCN Wizard
Nombre VCN: vcn_usuario
```
https://github.com/user-attachments/assets/7392fe58-7c60-40a3-8282-6900a0856a5e.mov

8. 
9.  
10. asd
11. a
12. sd
13. asd
