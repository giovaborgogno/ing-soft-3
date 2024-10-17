## Trabajo Práctico 8 - Implementación de Contenedores en Azure y Automatización con Azure CLI

### Desarrollo:

#### 4.1 Modificar nuestro pipeline para construir imágenes Docker de back y front y subirlas a ACR

##### 4.1.1 Crear archivos DockerFile para nuestros proyectos de Back y Front

![alt text](img/image.png)

##### 4.1.2 Crear un recurso ACR en Azure Portal siguiendo el instructivo 5.1

En mi caso ya habia creado uno para el proyecto final:
![alt text](img/image-1.png)

##### 4.1.3 Modificar nuestro pipeline en la etapa de Build y Test

- Luego de la tarea de publicación de los artefactos de Back agregar la tarea de publicación de nuestro dockerfile de back para que esté disponible en etapas posteriores:

![alt text](img/image-2.png)

- Luego de la tarea de publicación de los artefactos de Front agregar la tarea de publicación de nuestro dockerfile de front para que esté disponible en etapas posteriores:

![alt text](img/image-3.png)

##### 4.1.4 En caso de no contar en nuestro proyecto con una ServiceConnection a Azure Portal para el manejo de recursos, agregar una service connection a Azure Resource Manager como se indica en instructivo 5.2

Ya tenia creada una connection en mi repo:
![alt text](img/image-4.png)

##### 4.1.5 Agregar a nuestro pipeline variables

![alt text](img/image-5.png)

##### 4.1.6 Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Build y Test

- Agregar tareas para generar imagen Docker de Back

![alt text](img/image-6.png)

##### 4.1.7 - Ejecutar el pipeline y en Azure Portal acceder a la opción Repositorios de nuestro recurso Azure Container Registry. Verificar que exista una imagen con el nombre especificado en la variable backImageName asignada en nuestro pipeline

![alt text](img/image-7.png)

##### 4.1.8 - Agregar tareas para generar imagen Docker de Front (DESAFIO)

- A la etapa creada en 4.1.6 Agregar tareas para generar imagen Docker de Front

![alt text](img/image-8.png)

##### 4.1.9 - Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Construcción de Imagenes Docker y subida a ACR

- Agregar variables a nuestro pipeline:

![alt text](img/image-5.png)

- Agregar variable secreta cnn-string-qa desde la GUI de ADO que apunte a nuestra BD de SQL Server de QA como se indica en el instructivo 5.3

![alt text](img/image-9.png)

- Agregar tareas para crear un recurso Azure Container Instances que levante un contenedor con nuestra imagen de back

![alt text](img/image-10.png)

##### 4.1.10 - Ejecutar el pipeline y en Azure Portal acceder al recurso de Azure Container Instances creado. Copiar la url del contenedor y navegarlo desde browser. Verificar que traiga datos.

![alt text](img/image-11.png)

giovaborgogno-crud-backend-container-qa.eastus.azurecontainer.io

![alt text](img/image-12.png)

##### 4.1.11 - Agregar tareas para generar un recurso Azure Container Instances que levante un contenedor con nuestra imagen de front (DESAFIO)

- A la etapa creada en 4.1.9 Agregar tareas para generar contenedor en ACI con nuestra imagen de Front

![alt text](img/image-13.png)

- Tener en cuenta que el contenedor debe recibir como variable de entorno API_URL el valor de una variable container-url-api-qa definida en nuestro pipeline.

![alt text](img/image-14.png)

- Para que el punto anterior funcione el código fuente del front debe ser modificado para que la url de la API pueda ser cambiada luego de haber sido construída la imagen. Se deja un ejemplo de las modificaciones a realizar en el repo https://github.com/ingsoft3ucc/CrudAngularConEnvironment.git

![alt text](img/image-15.png)
![alt text](img/image-16.png)

##### 4.1.12 - Agregar tareas para correr pruebas de integración en el entorno de QA de Back y Front creado en ACI.

![alt text](img/image-17.png)
![alt text](img/image-18.png)
![alt text](img/image-19.png)

#### 4.4 Desafíos:

- 4.4.1 Agregar tareas para generar imagen Docker de Front. (Punto 4.1.8) ✅
- 4.4.2 Agregar tareas para generar en Azure Container Instances un contenedor de imagen Docker de Front. (Punto 4.1.11) ✅
- 4.4.3 Agregar tareas para correr pruebas de integración en el entorno de QA de Back y Front creado en ACI. (Punto 4.1.12) ✅
- 4.4.4 Agregar etapa que dependa de la etapa de Deploy en ACI QA y genere contenedores en ACI para entorno de PROD. ✅

Creamos un environment para Production con manual approvals para el deployment

![alt text](img/image-21.png)
![alt text](img/image-20.png)
![alt text](img/image-22.png)
![alt text](img/image-23.png)

Pipeline:
https://dev.azure.com/giovaborgogno/Angular_WebAPINetCore8_CRUD_Sample/_build/results?buildId=67&view=results