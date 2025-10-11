Usando MongoDB con Docker: Un Ejemplo Práctico

Voy a guiarte paso a paso para usar MongoDM con Docker y crear un ejemplo práctico que te ayude a entender cómo funciona esta base de datos NoSQL.



Paso 1: Instalar Docker

Primero, asegúrate de tener Docker instalado en tu sistema:



Descargar Docker Desktop para Windows/Mac



Para Linux, usa el gestor de paquetes de tu distribución (ej. sudo apt-get install docker-ce para Ubuntu)



Paso 2: Ejecutar MogoDB en un contenedor Docker

Abre tu terminal y ejecuta:



docker run -d --name mongodb -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=admin mongodb/mongodb-community-server:latest



¿que hace este comando?

Descarga la imagen oficial de Mongo (si no la tienes)

Crea y ejecuta un contenedor llamado "mongodb"

Expone el puerto 27017(puerto predeterminado de Redis)

Y crea un usuario admin con contraseña admin

Lo ejecuta en segundo plano (-d)

No recomendado, pero podemos configurar MongoDB sin usuario ni contraseña

docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest



Paso 3: Conectarte al servidor MongoDB

Puedes interactuar con Mongo de dos formas:



Opción 1: Consola de mongo desde dentro del contenedor (el que vamos a usar)

Desde el terminal ejecuta:

docker exec -it mongodb mongosh --port 27017 -u admin -p admin --authenticationDatabase admin



Opción 2: Instalar mongo compass e interactuar

La URL de conexión es: mongodb://admin:admin@localhost:27017/

Una vez instalado, desde un terminal ejecutas:

Paso 4: Parar MongoDB

Desde terminal, ejecuta:
docker stop mongodb

Paso 5 : Iniciar MongoDB
Asegúrate que tienes abierto docker

Desde terminal, ejecuta:
docker start mongodb

Y luego conectarse (repetir el paso 3)