# Workshop Container Instances

### Requerimientos:

- Cuenta de Oracle Cloud Infrastructure(test gratuito https://www.oracle.com/cloud/free/)
- Cuenta de Github (https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
- Poseer servidor linux con las siguientes dependencias instaladas:
  - git
  - python36-oci-cli
  - podman - docker 


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

```
Una vez creada la VCN configurar security list para subnet privada y pública, en ambos casos se debe crear regla de ingress
Dentro de la subnet privada crear la siguiente regla:
Security List > security list for private subnet-vcn-fbasso > Add Ingress Rules
Source CIDR: 0.0.0.0/0
IP Protocol: TCP
Destination Port Range: 80

Dentro de la red pública
Security List > Default Security List for vcn-fbasso > Add Ingress Rules
Source CIDR: 0.0.0.0/0
IP Protocol: TCP
Destination Port Range: 80
```

https://github.com/user-attachments/assets/2e60bb32-b2de-4fb3-8d8c-e4c3d00e0d5f

8. Creación de Container Instance
```
Menú Principal > Developer Servicess > Container Instances > Create container instance
Name: usuarioInstance
Shape: Ampere
OCPU: 1
Memory: 2 Gb
Networking: vcn-usuario
Subnet: Private Subnet
NEXT

Container-1
Name: containerUsuario
Image: Selecionamos la creada
Slecionamos la opción: Provide credentials manually
Registry User: NAMESPACE/USUARIO
Registry password: Token Creado Previamente
NEXT

Create Instance y esperamos a que se despliegue la instancia
```
<img width="818" alt="image" src="https://github.com/user-attachments/assets/67a7ec26-ad73-4164-8434-10abe2aa37ae">

https://github.com/user-attachments/assets/1de7a6ec-8590-4f36-a85c-ffbaf0b583d4

9. Configurar cloud shell para prueba de instancia
```
Developer Tool > cloud Shell
Desde la cloud Shell en la opción "Network:" crear una conexión efimera que apunte a la red privada de la VCN creada
```
<img width="949" alt="image" src="https://github.com/user-attachments/assets/0ec837dc-ceb4-49b8-84ed-6ac3f8a9628e">

https://github.com/user-attachments/assets/15d3ffb3-e2d2-4b44-b6ec-1e9d4746a91c

10. Una vez configurada cloud shell probar la conexión 
```
Desde la información de la instancia copiar la ip privada, en este ejemplo es al 10.0.1.218 y desde la consola ejecutar
$ curl 10.0.1.218:80
Deberíamso tener un resultado similar a este:

<html>
  <head>
    <title>Deploy Demo</title>
    <style>
      body {
        background-color: lightblue;
      }
    </style>
  </head>
  <body>
    <h2>Servicio 1 desde localhost!</h2>
  </body>
</html>
```
https://github.com/user-attachments/assets/806e0380-911e-4603-a576-6d752259ff01

11. Para hacer pública la isntancia se debe crear un load balancer
```
Menú Principal > Networking > Load Balancers > Load Balancer > Create load balancer
Definir:
Nombre: lb_instanciaUsuario (en mi caso solo lb_instancia)
Choose networking
Virtual cloud network: vcn-USUARIO
Subnet: pubic subnet-vcn-USUARIO
Compartment: USUARIO
NEXT

En la segunda ventana no cambiar nada
NEXT

En la tercera ventana cambiar:
Specify the type of traffic your listener handles: HTTP
Specify the port your listener monitors for ingress traffic: 80
NEXT

En la cuarta ventana selecionar la opción Create a new log group
Habilitar Access logs
selecionar la opción: Create a new log group
NEXT

Submit

Esperar un poco y validar la creación de load balancer
```
https://github.com/user-attachments/assets/c2ac36fb-bcaa-4143-a9c9-d8d6db52bd55

12. Una vez creado el load balancer se debe configurar el backendset con la ip de la instancia
```
Load Balancer > Backend sets > bs_lb_2024-XXX-XXX > Backends > Add backends

Definir la opción IP addresses, pegar la ip de la instancia en IP address y Port 80
Luego, click en Add

esperar un poco y ver que el estado cambie a OK
```
https://github.com/user-attachments/assets/8150cdde-9ead-4721-84cf-0f2ec1316671

13. Finalmente y para validar que la instancia funcione como corresponde, validar la ip pública del balanceador y acceder desde el navegador web

https://github.com/user-attachments/assets/28e05af2-95a9-4ebc-a342-ffa6a350ef5b

<img width="506" alt="image" src="https://github.com/user-attachments/assets/824d0e4d-0de8-4da7-a737-b5982427b1e4">

