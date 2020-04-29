![FIWARE Banner](https://fiware.github.io/tutorials.IoT-Sensors/img/fiware.png)[<img src="https://raw.githubusercontent.com/FIWAREZone/misc/master/Group%400%2C36x.png"  align="right">](http://www.fiware.zone)



# Despliegue de Context Broker y uso de la plataforma Thinking Cities
Este tutorial es una introducción al despliegue de un Context Broker en un entorno local y el uso de la plataforma Thinking Cities de Telefónica como plataforma comercial FIWARE.

# Prerequisitos

## Postman

Postman es un cliente de peticiones HTTP, que nos permitirá interactuar con la API NGSI de una forma gráfica, sencilla e intuitiva. Para obtener más información sobre como instalar y configurar Postman, así como para descargar la colección de peticiones y el fichero de variables de entorno, haz click [aquí](postman/README.md). También puedes descargar los ficheros a continuación:

- [Colección de peticiones]()
- [Fichero de entorno]()

## Entorno donde correr Docker

Existen varios métodos para poder ejecutar docker en su equipo. Si usted está utilizando una distribución Linux, seguramente pueda instalar sin ningún tipo de problema Docker de forma Nativa. 

A continuación se enlazan las guías de instalación de docker para los distintos sistemas operativos:
- **Linux**. Dependiendo de la distribución tienen guías distintas:
   - **Centos** https://docs.docker.com/install/linux/docker-ce/centos/
   - **Debian** https://docs.docker.com/install/linux/docker-ce/debian/
   - **Ubuntu** https://docs.docker.com/install/linux/docker-ce/ubuntu/

- **Windows**: https://docs.docker.com/docker-for-windows/install/
- **MacOS**: https://docs.docker.com/docker-for-mac/install/

Para que el tutorial sea lo más homogéneo posible, vamos a instalar docker y docker-compose sobre una máquina virutal. Así, la ejecución de este tutorial será totalmente independiente del sistema operativo (huesped) y de la máquina en la que se corre.

# Instalación máquina virtual con Linux

## Descarga e instalación de Virtualbox

## Descarga de Centos y creación de la máquina virtual

En este caso queremos que la propia máquina virtual tenga una IP propia, por lo que en configuraciónes de red vamos a elegir la opción *****************

![Importar Environment](images/image2.png)

## Conexión a la máquina virtual y primeros pasos
Dado que Centos no se conecta de forma automática a la red, vamos a ejecutar el cliente DHCP para que la máquina obtenga IP.

```console
dhclient
```

A continuación vamos a comprobar la IP que tiene la máquina virtual para poder conectarnos por SSH.

```console
ip addr
```

Una vez conocemos la IP de la máquina, abrimos una sesión SSH, para ello, en Windows podemos usar Símbolo de Sistema o PowerShell. Tanto en Linux como en MacOS, podemos usar la consola de comandos.

```console
ssh user@domain
```

Donde `user` será el usuario con el que hemos creado la cuenta al hacer la instalación del sistema operativo y `domain` la IP que hemos obtenido en el paso anterio.

Una vez tengamos acceso, aprovechamos para instalar wget, que será utilizado más adelante para descargar el fichero docker-compose.yml, de la siguiente manera

```console
yum install wget
```


## Instalación de Docker y Docker-Compose en la máquina virtual

Es necesario disponer de Docker y Docker compose dado que vamos a lanzar los servicios contenedorizados. Una vez podemos acceder a la máquina debemos seguir los siguientes pasos:

### Instalar instalar docker y lanzar servicio. 
Primero instalamos docker

```console
yum install docker
```
Lanzamos los servicios de sistema

```console
systemctl enable docker
systemctl start docker
```
Comprobamos que Docker funciona correctamente

```console
docker info
```

### Instalar docker-compose

Descargamos Docker-Compose y le damos permisos de ejecución:

```console
sudo curl -L “https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

# Desplegando Orion Context broker y probando su API

Antes de nada debemos descargarnos el fichero docker compose con el que vamos a desplegar el context broker

```console
wget https://raw.githubusercontent.com/telefonicaid/fiware-orion/master/docker/docker-compose.yml
```
El fichero que acabamos de descargar es el siguiente:

```yml
mongo:
  image: mongo:3.6
  command: --nojournal

orion:
  image: fiware/orion
  links:
    - mongo
  ports:
    - "1026:1026"
  command: -dbhost mongo

```

En el podemos ver los 2 contenedores que se van a desplegar, por un lado Orion, y por otro MongoDB, que es la base de datos que emplea el Orion Context Broker para la persistencia de los datos.

Para lanzar los contenedores solo tenemos que ejecutar el siguiente comando, siempre y cuando estemos en el mismo directorio que el fichero docker-compose.yml que queremos lanzar

```console
docker-compose up
```

Alternativamente, para lanzarlo en segundo plano, su puede usar:

```console
docker-compose up -d
```


## Comprobar que Orion funciona correctamente
Desde el navegador
Acceder a http://{{IP_VM}}:1026/version

Desde el terminal, en la propia máquina virtual, podemos comprobarlo de la siguiente manera:

```console
curl -X GET  'http://localhost:1026/version'
```

## Interactuando con la API

### Creación de una entidad

Vamos a crear nuestra primera entidad en el context broker. Esta entidad será de tipo coche o `Car`, con el identificador `entity-id:001` y con 3 atributos: 
- `Brand` de tipo texto, con valor `Seat`
- `Model` de tipo texto, con valor `Leon`
- `Name` de tipo texto, con valor `Vehículo de antonio` 

Para ello ejecutamos el siguiente comando:

```console
curl -iX POST 'http://localhost:1026/v2/entities' \
-H 'Content-Type: application/json' \
-d '
{
    "id": "entity-id:001",
    "type": "Car",
    "location": {
        "type": "geo:json",
        "value": {
             "type": "Point",
             "coordinates": [13.3986, 52.5547]
        }
    },
    "Brand": {
        "type": "Text",
        "value": "Seat"
    },
    "Model": {
        "type": "Text",
        "value": "Leon"
    },
    "Name": {
        "type": "Text",
        "value": "Vehículo de Antonio"
    }
}'
```

El terminal nos devolvera la siguiente respuesta:

```
HTTP/1.1 201 Created
Connection: Keep-Alive
Content-Length: 0
Location: /v2/entities/entity-id:001?type=Car
Fiware-Correlator: 4905034e-7f3b-11ea-b71c-0242ac120003
Date: Wed, 15 Apr 2020 17:05:19 GMT
```

### Añadir un atributo a la entidad





### Lectura de las entidades almacenadas
```console
curl -iX GET 'http://localhost:1026/v2/entities'
```

```console
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Length: 313
Content-Type: application/json
Fiware-Correlator: 7ab153e8-7f3b-11ea-80ad-0242ac120003
Date: Wed, 15 Apr 2020 17:06:42 GMT

[{"id":"entity-id:001","type":"Car","Brand":{"type":"Text","value":"Seat","metadata":{}},"Model":{"type":"Text","value":"Leon","metadata":{}},"Name":{"type":"Text","value":"Vehículo de Antonio","metadata":{}},"location":{"type":"geo:json","value":{"type":"Point","coordinates":[13.3986,52.5547]},"metadata":{}}}]
```

```json
[
  {
    "id": "entity-id:001",
    "type": "Car",
    "Brand": {
      "type": "Text",
      "value": "Seat",
      "metadata": {}
    },
    "Model": {
      "type": "Text",
      "value": "Leon",
      "metadata": {}
    },
    "Name": {
      "type": "Text",
      "value": "Vehículo de Antonio",
      "metadata": {}
    },
    "location": {
      "type": "geo:json",
      "value": {
        "type": "Point",
        "coordinates": [
          13.3986,
          52.5547
        ]
      },
      "metadata": {}
    }
  }
]```





