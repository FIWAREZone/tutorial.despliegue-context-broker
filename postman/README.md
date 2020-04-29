
# Postman
Colección de peticiones y variables de entorno

# Introducción

Para hacer pruebas con la API de FIWARE será necesario disponer de una herramienta que permita hacer peticiones HTTP e interactuar con ésta. En este caso, recomendamos la herramienta Postman ya que permite gestionar y configurar de forma sencilla una colección de peticiones, por lo que será muy sencillo realizar las operaciones necesarias.

Postman nos permite definir un catálogo de peticiones ([Collection](FIWARE%20Zone%20Course.postman_collection.json)) y variables de entorno ([Environment](FIWARE%20Zone%20Course%20Enviroment%20Template.postman_environment.json)) que, posteriormente, podemos exportar e importar para compartirlas con otras personas. 

# Instalación y configuración de Postman

1. Descargar [Postman](https://www.getpostman.com/) e instalar.

2. Descargar el fichero ZIP con el Collection (peticiones) y el Environment (variables) que se van a utilizar en la práctica. [Enlace al fichero ZIP](https://github.com/FIWAREZone/IoT_Course/blob/master/postman/Postman_Collection%26Environment.zip) (hacer click sobre la opción Download)

![Descargar ZIP](images/download_zip.jpg)

3. En este punto debemos tener Postman instalado y dos ficheros con extensión ".json" en nuestro PC.

4. Abrir Postman.

5. Importar la colección "FIWARE Zone Course.postman_collection.json"

![Importar Collection](images/import_collection.jpg)

6. Importar el environment "FIWARE Zone Course Enviroment Template.postman_environment.json"

![Importar Environment](images/import_environment.jpg)

7. Configurar las credenciales: user, password, service y subservice (Service Path) que serán indicados durante la realización del curso de formación.



## Variables de entorno

Las variables de entorno facilitan la gestión y modificación de los parámetros que se usan en las colecciones, como por ejemplo la URL del servidor en el que se encuentra instalada la plataforma FIWARE. Por ello, es necesario modificar las variables de entorno con los parámetros correspondientes:

| Parametro         |descripción   												|
| :-----------------|:--------------											|
| user              | Usuario con permisos de acceso							|
| password          | Contraseña del Usuario 									|
| service   		| Servicio del contex broker (Header Fiware-Service) 		|
| subservice      	| Subservicio del context broker (Header Fiware-ServicePath) |
| token 			| Token de autenticación de usuario dinámico 				|
| host 				| URL del servidor, es decir, del Orion Context Broker 		|
| host_iota 				| URL del servicio IoT Agent 		|
| iota_port 				| Puerto del servicio IoT Agent		|
| UL_apikey 				| Apikey para el envío de datos por IoTA con protocolo Ultralight 2.0 		|
| JSON_apikey 			| Apikey para el envío de datos por IoTA con protocolo JSON 		|
| device_ID 				| Identificador de dispositivo		|
