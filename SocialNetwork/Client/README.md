# SoftUni Social Network - frontend

El proyecto abarca el desarrollo de una aplicación web orientada exclusivamente a la comunidad de una institución educativa, como la Universidad Tecnológica Nacional, permitiendo a sus miembros registrarse, crear y administrar su perfil, establecer conexiones con otros usuarios, intercambiar mensajes privados y compartir publicaciones e imágenes dentro del entorno institucional. La plataforma estará enfocada en facilitar la comunicación y la interacción social entre estudiantes y demás integrantes de la universidad, funcionando como un complemento al Campus Virtual tradicional. El proyecto contempla las funcionalidades principales de una red social interna, pero no incluye la gestión académica formal (como inscripción a materias, carga de notas o administración de cursadas), ya que su propósito está centrado en la comunicación y la integración de la comunidad universitaria.

---

## 📌 Tabla de Contenidos

- [Características](#características)
- [Tecnologías](#tecnologías)
- [Requisitos](#requisitos)
- [Instalacion](#despliegue)

---

## 🚀 Características

- Crear nuevo usuario
- Envio de mensajes 
- Solicitud de amistad
- Notificaciones en tiempo real
- Subir imagenes a la seccion del usuario

---

## 🛠 Tecnologías

- React
- Node Package manager 
- Docker
---

## 📋 Requisitos

Antes de ejecutar el proyecto necesitás tener instalado:
- React
- Node Package manager 11+
- Docker / Docker Desktop

## Ejecucion en desarrollo 

Dentro de nuestra terminal de comando, damos de alta la variabole de entorno que apunta al backend
 ```linux
 back_end_uri = <URL DEL BACK END>
 ```
 Luego de ingresar la variable de entorno, ejecutamos el codigo
 ``` linux
 npm start
 ```
 
## ⚙️ Despliegue

 ### Creacion de imagen Docker:

```docker
 docker build -t network_front_v3 .
 ```
 
 ### Creamos etiqueta para el proyecto
 ```docker
 docker tag network_front_v3:latest 766749599583.dkr.ecr.us-east-1.amazonaws.com/social-network-frontend:v3 
 ```

 ### Enviamos imagen creada al servicio de AWS ECR con nuestro usario IAM con permisos anteriormente creado
 ```docker
 docker push 766749599583.dkr.ecr.us-east-1.amazonaws.com/social-network-frontend:v3
 ```

### Verificamos que la imagen ya se encuentra en AWS
![alt text]({4BB5B4BA-4059-4186-AE0C-BDD2376D24CA}.png)

### Creacion de instancias ECS

#### Nos dirigimos hacia el servicio de ECS principalmente a la seccion de "Definicion de tareas" y hacemos clic en "Crear una nueva definicion de tarea"
![alt text]({C2D2F65C-C5D0-49D3-9C65-DB4062220E85}.png)

### Llenamos el formulario con los requisitos necesarios 

#### Ingresamos el nombre de la familia que pertenecerá tarea a crear
![alt text]({C8630A31-5373-4220-8647-C5B32FC9E734}.png)

#### La instancia la definimos como tipo Fargate o contenedores si servidor
![alt text]({63F20F52-A2BD-4B10-BA8C-74FB28D73C46}.png)

#### Le indicamos la imagen a usar, haciendo en el explorador de imagenes de ECR o copiamos y pegamos la URI de la imagen
![alt text]({63039CC4-5C8A-4906-9EA5-2E0827E73DB1}.png)

#### ⚠️ !IMPORTANTE: Mapear los puertos en donde se ejecutan las aplicaciones y definirlas en la instancia. En este caso es un codigo escrito en React, por defecto se usa el puerto 3000
![alt text]({B6911D9C-F480-44A4-B278-FEFDD58C1E7A}.png)

#### ⚠️ !IMPORTANTE: Ingresar las variables de entorno que son necesarias, en este caso la URI que apunta al backned (Se recomienda tener el servicio de Backend deplegado antes que el Frontend)
![alt text]({506B16C0-98C1-45F9-8E94-3FADBF91F46A}.png)

#### ✅ Finalizado estos, podremos hacer clic en "Crear"

### Creacion de Cluster para la ejecucion de las tareas creadas

#### Nos dirigimos a la seccion de "Clusteres" y hacemos click en "Crear cluster"

#### llenamos el fomulario empezando por agregar un nombre 
![alt text]({7935A41C-AAFD-42BC-B9E5-25200E183927}.png)

#### Para el tipo de infraestructura volvemos a indicarle que sea solo de tipo "Fargate"
![alt text]({CBA389FE-395D-4B3E-9ADC-0D8A4E486BB4}.png)

#### ✅ Finalizado esto, podremos hacer clic en "Crear"
![alt text]({E3DF1736-651A-4B19-A86B-5B7B6E17C861}.png)

### Definicion de la Tarea a ejecutar en el Cluster.

#### Ingresamos al cluter creado, haciendo click en el nombre dentro de la lista de clusteres creados.
![alt text]({22A04485-0DBF-4B86-B630-5231F291425C}.png)

#### Nos dirigimos hacia la seccion de tareas y hacemos clic en "Ejecutar nueva tarea"
![alt text]({389B2B5C-07A5-4AC0-A662-1500B7D37FB3}.png)

#### Dentro de ese apartado , seleccionamos la familia de tareas a ejecutar
![alt text]({4A125485-1DC1-4296-93F7-0CFC0918A2ED}.png)

#### La cantidad de tareas a lanzar depende del usuario, en este caso se ejecutará solo una, por lo que solo se creara 1 usa sola puera de enlace publica al servicio
![alt text]({3FF28F6C-1EFA-4EB3-80F8-C6304C3B7325}.png)

#### ⚠️ Es inportante que en el apartado de red definan bien VPC y la subred en la que se podrá acceder al servicio, en este caso se utlizará la VPC por defecto que viene creada en AWS y la subnet "us-east-1a"
![alt text]({0603CCDE-E080-447A-8E7A-23EDD71080A9}.png)

#### ⚠️ Es importante tambien definir a que grupo de seguridad pertenecerá esta instancia, ya que por temas de reglas de firedwall, podriamos tener impedimento el poder acceder al servicio desde internet
![alt text]({E91B8DAC-C060-4E30-AA7A-47835D0AF3DB}.png)

✅ Finalizado esto, podremos hacer clic en el boton de "Crear"
![alt text]({737ABD75-ACA2-41C3-9A8D-CFDCFA1DF5E1}.png)

![alt text]({95F90BAE-D870-4102-8135-3179B0F74DC8}.png)ç


✅ Finalizado estos pasos, dentro de la tarea podremos entrar a nuestra web desplegada via la ip publica creada al crear la tarea.

![alt text]({F1D6FC3B-A627-4806-8BB5-B9C5F0D2E9A6}.png)
![alt text]({A74C5DAC-BDDB-4E9D-9A48-24843FFABCE6}.png)