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

 
5. Conectar por ssh a la instancia 129.159.26.160, las credenciales serán entregadas en el laboratorio

7. Creación de imágen, ejecutar los siguientes comandos:
```
$ cd Docker/
$ podman build --tag NOMBRE_REGISTRY -f Dockerfile                  # En mi caso es podman build --tag app1fbasso -f Dockerfile
$ podman images
```



https://github.com/user-attachments/assets/ed6119b4-a43c-46f2-b865-d01e4f7681c4


8. 
9. asd
10. a
11. sd
12. asd
