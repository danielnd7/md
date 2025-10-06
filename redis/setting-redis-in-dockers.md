### Usando Redis con Docker: Un Ejemplo Práctico

Voy a guiarte paso a paso para usar Redis con Docker y crear un ejemplo práctico que te ayude a entender cómo funciona esta base de datos NoSQL.



Paso 1: Instalar Docker

Primero, asegúrate de tener Docker instalado en tu sistema:



Descargar Docker Desktop para Windows/Mac



Para Linux, usa el gestor de paquetes de tu distribución (ej. sudo apt-get install docker-ce para Ubuntu)



Paso 2: Ejecutar Redis en un contenedor Docker

Abre tu terminal y ejecuta:



docker run --name mi-redis -p 6379:6379 -d redis



¿que hace este comando?

Descarga la imagen oficial de Redis (si no la tienes)

Crea y ejecuta un contenedor llamado "mi-redis"

Expone el puerto 6379 (puerto predeterminado de Redis)

Lo ejecuta en segundo plano (-d)



Paso 3: Conectarte al servidor Redis

Puedes interactuar con Redis de dos formas:



Opción 1: CLI de Redis dentro del contenedor (el que vamos a usar)

Desde el terminal ejecuta:

docker exec -it mi-redis redis-cli



Opción 2: Instalar redis-cli localmente e interactuar

Si tienes redis-cli instalado localmente (o quieres instalarlo), puedes conectarte directamente:



https://redis.io/docs/latest/operate/oss\_and\_stack/install/archive/install-redis/



Una vez instalado, desde un terminal ejecutas:

redis-cli -h localhost -p 6379



Paso 4: Parar Redis

Desde terminal, ejecuta:
docker stop mi-redis

Paso 5 : Iniciar Redis
Asegúrate que tienes abierto docker

Desde terminal, ejecuta:
docker start mi-redis

Y luego conectarse (repetir el paso 3)




--------------------------------------------------------
in class:

docker start mi-redis

docker ps

docker exec -it mi-redis redis-cli  // to execute cli of redis

select ....  (select 0) // to change to another db

exit // to exit from db

docker stop mi-redis // to stop the container


------------------------------------------------------


docker exec -it mi-redis redis-cli

	127.0.0.1:6379> set asignatura "Base de datos"
	OK

	127.0.0.1:6379> get asignatura
	"Base de datos"

	127.0.0.1:6379> mset cantante1 "Perico" 
	OK

	127.0.0.1:6379> mset cantante2 "Juan"
	OK
	
	127.0.0.1:6379> keys *
	1) "cantante2"
	2) "asignatura"
	3) "cantante1"

	
	127.0.0.1:6379> keys c*
	1) "cantante2"
	2) "cantante1"

	127.0.0.1:6379> set capitulo_actual "capitulo 1"
	OK

	127.0.0.1:6379> TTL capitulo_actual
	(integer) -1

	127.0.0.1:6379> EXPIRE capitula_actual 20
	(integer) 0

	127.0.0.1:6379> ttl capitulo_actual
	(integer) -1

	127.0.0.1:6379> get capitulo_actual
	"capitulo 1"

	127.0.0.1:6379> ttl capitulo_actual
	(integer) -1

	127.0.0.1:6379> get capitulo_actual
	"capitulo 1"

	127.0.0.1:6379> set capitulo_actual "capitulo" EX 10
	OK

	127.0.0.1:6379> get capitulo_actual
	"capitulo"

	127.0.0.1:6379> ttl capitulo_actual
	(integer) -2

	127.0.0.1:6379> get capitulo_actual
	(nil)




