# SoftUni Social Network - backend

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

- Java 11
- Spring Boot
- Spring cloud
- Maven 
- Hibernate y JPA
- MySQL
- Docker
- Swagger / OpenAPI
- Cloudinary
---

## 📋 Requisitos

Antes de ejecutar el proyecto necesitás tener instalado:
- Java 11+
- Maven 3.1+
- Base de datos configurada
- Git
- Cloudinary api key
- Docker / Docker Desktop

## Ejecucion en desarrollo 

Dentro de nuestra terminal de comando, damos de alta la variabole de entorno que apunta al backend
 ```linux
  spring_service_type = <home,gateway,chat,cloudinary>
  db_host = <URL DE LA BASE DE DATOS>
  db_password = <PASSWORD>
  db_username = <USERNAME>
  home_service_url = <url_home_service>
  chat_service_url = <url_chat_service>
  cloudinary_service_url = <url_cloudinary_service>
 ```
 Luego de ingresar la variable de entorno, ejecutamos el codigo
 ``` linux
 mvn spring-boot:run
 ```
 
## ⚙️ Despliegue

 ### Creacion de imagen Docker:

```docker
 docker build -t network_back_v1 .
 ```
 
 ### Creamos etiqueta para el proyecto
 ```docker
 docker tag network_backend_v1:latest 766749599583.dkr.ecr.us-east-1.amazonaws.com/social-network-backend:v1 
 ```

 ### Enviamos imagen creada al servicio de AWS ECR con nuestro usario IAM con permisos anteriormente creado
 ```docker
 docker push 766749599583.dkr.ecr.us-east-1.amazonaws.com/social-network-backend:v1
 ```

### Verificamos que la imagen ya se encuentra en AWS
<img width="1578" height="461" alt="{C4858593-5A94-4A30-BD05-3B1F71D83717}" src="https://github.com/user-attachments/assets/e7a9b191-9f28-4095-a0ed-0b6d0a9af333" />

### Creacion de instancias ECS

#### Nos dirigimos hacia el servicio de ECS principalmente a la seccion de "Definicion de tareas" y hacemos clic en "Crear una nueva definicion de tarea"
<img width="1908" height="261" alt="image" src="https://github.com/user-attachments/assets/8106c5c4-3597-4a79-ae8b-d9de6e3eb0e3" />

### Llenamos el formulario con los requisitos necesarios 

#### Ingresamos el nombre de la familia que pertenecerá tarea a crear
<img width="1097" height="156" alt="image" src="https://github.com/user-attachments/assets/6f5bac58-1eb1-4ed0-a33d-b9c5b715adbe" />

#### La instancia la definimos como tipo Fargate o contenedores si servidor
<img width="1048" height="267" alt="image" src="https://github.com/user-attachments/assets/b03fe5f5-d587-451d-890e-7a30b6bf8097" />

#### Le indicamos la imagen a usar, haciendo en el explorador de imagenes de ECR o copiamos y pegamos la URI de la imagen
<img width="1336" height="142" alt="image" src="https://github.com/user-attachments/assets/eed4f60b-e984-451f-9d13-146dcfadef21" />

#### ⚠️ !IMPORTANTE: Mapear los puertos en donde se ejecutan las aplicaciones y definirlas en la instancia. En este caso es un codigo escrito en Java con Spring boot, por defecto se usa el puerto 8080
<img width="1287" height="209" alt="{F0B0F9DD-1672-4363-84CF-099FFF836E11}" src="https://github.com/user-attachments/assets/00241c47-e584-438b-9e4b-de49b8de102e" />

#### ⚠️ !IMPORTANTE: Ingresar las variables de entorno ya que con ella le decimos que partes del servicios van a funcionar y cual no, el tipo de servicio que va a ser y las credenciales de base de datos.
<img width="967" height="524" alt="{DAE17173-5DBA-462D-A2C1-438D94CB32EA}" src="https://github.com/user-attachments/assets/e7f9a53c-bb9c-4be7-ad87-089016205ae2" />


#### ✅ Finalizado estos, podremos hacer clic en "Crear"

### Creacion de Cluster para la ejecucion de las tareas creadas

#### Nos dirigimos a la seccion de "Clusteres" y hacemos click en "Crear cluster"

#### llenamos el fomulario empezando por agregar un nombre 
<img width="1319" height="222" alt="image" src="https://github.com/user-attachments/assets/e8f4e1e8-8e87-49b2-8968-8969592dff7b" />

#### Para el tipo de infraestructura volvemos a indicarle que sea solo de tipo "Fargate"
<img width="628" height="216" alt="image" src="https://github.com/user-attachments/assets/440c688f-bf77-4160-aa6f-354f648665a2" />

#### ✅ Finalizado esto, podremos hacer clic en "Crear"

### Definicion de la Tarea a ejecutar en el Cluster.

#### Ingresamos al cluter creado, haciendo click en el nombre dentro de la lista de clusteres creados.
<img width="1575" height="36" alt="image" src="https://github.com/user-attachments/assets/58c6c4ac-03ae-44f2-b526-cc099326cdc9" />

#### Nos dirigimos hacia la seccion de tareas y hacemos clic en "Ejecutar nueva tarea"
<img width="1588" height="123" alt="image" src="https://github.com/user-attachments/assets/3dd0bdf3-2808-4613-a63e-53d7d0497fa9" />

#### Dentro de ese apartado , seleccionamos la familia de tareas a ejecutar
<img width="1077" height="225" alt="image" src="https://github.com/user-attachments/assets/59ab65f5-5cd1-4505-9cf5-c012e3a5827c" />

#### La cantidad de tareas a lanzar depende del usuario, en este caso se ejecutará solo una, por lo que solo se creara 1 usa sola puera de enlace publica al servicio
<img width="1106" height="88" alt="image" src="https://github.com/user-attachments/assets/d7577952-5862-473c-8b47-ba269922d606" />

#### ⚠️ Es inportante que en el apartado de red definan bien VPC y la subred en la que se podrá acceder al servicio, en este caso se utlizará la VPC por defecto que viene creada en AWS y la subnet "us-east-1a"
<img width="1370" height="358" alt="image" src="https://github.com/user-attachments/assets/a4f8ab82-e4fb-4bb9-9ab7-6c488e5351c4" />



#### ⚠️ Es importante tambien definir a que grupo de seguridad pertenecerá esta instancia, ya que por temas de reglas de firewall, podriamos tener impedimento el poder acceder al servicio desde internet
<img width="1176" height="211" alt="image" src="https://github.com/user-attachments/assets/46aa2ed6-cdf1-4ffa-ae61-8fa2e9ee9613" />


✅ Finalizado esto, podremos hacer clic en el boton de "Crear"
<img width="203" height="54" alt="image" src="https://github.com/user-attachments/assets/8f501542-779d-4a32-a4ba-f960abca3af1" />
<img width="1593" height="238" alt="image" src="https://github.com/user-attachments/assets/f3e00367-f660-4fe9-9c2d-61c633934192" />

✅ Finalizado estos pasos, dentro de la tarea podremos verificar via internet si el servicio backend está corriendo.

<img width="888" height="322" alt="{93445AAD-77A1-4971-9E60-D9F0608FCA11}" src="https://github.com/user-attachments/assets/70d9452b-9385-4214-a2f9-fb3a70cd8b08" />

⚠️Es importante aclarár que al implementar spring security en nuestro codigo y la autenticacion por token, es comun que al intentar ingresar al back via navegado, esta sea rechazado por no haber iniciado seccion y haber pasado un token  por el heather de autentizacion. Error 403 Forbidden.
<img width="1222" height="277" alt="{A2296112-2A79-4F4F-A19B-1C2E6F8EE121}" src="https://github.com/user-attachments/assets/6d04d55a-a4b3-418e-a366-6e5a7c2adb0b" />



