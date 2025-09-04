# App usando contenedores

Una aplicacion simple usando contenedores

## Getting started

Descarga Docker Desktop
 para Mac o Windows. Docker Compose
 se instalará automáticamente. En Linux, asegúrate de tener la última versión de Compose
.

Esta solución utiliza Python, Node.js, .NET, con Redis para mensajería y Postgres para almacenamiento.

Ejecuta en este directorio para construir y ejecutar la aplicación:

docker compose up


La aplicación vote estará disponible en http://localhost:8080
, y la aplicación results estará en http://localhost:8081
.

De manera alternativa, si deseas ejecutarla en un Docker Swarm
, primero asegúrate de tener un swarm. Si no lo tienes, ejecuta:

docker swarm init


Una vez que tengas tu swarm, en este directorio ejecuta:

docker stack deploy --compose-file docker-stack.yml vote

## Run the app in Kubernetes

La carpeta k8s-specifications contiene las especificaciones YAML de los servicios de la Aplicación de Votación.

Ejecuta el siguiente comando para crear los deployments y servicios. Ten en cuenta que estos recursos se crearán en tu namespace actual (default si no lo has cambiado).

kubectl create -f k8s-specifications/


La aplicación web vote estará disponible en el puerto 31000 de cada host del clúster, y la aplicación web result estará disponible en el puerto 31001.

Para eliminarlos, ejecuta:

kubectl delete -f k8s-specifications/

## Arquitectura


* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Notas

La aplicación de votación solo acepta un voto por navegador de cliente. No registra votos adicionales si ya se envió uno desde el mismo cliente.

Este no es un ejemplo de una aplicación distribuida perfectamente diseñada… es solo un ejemplo simple de los diferentes tipos de componentes y lenguajes que podrías encontrar (colas, datos persistentes, etc.), y cómo manejarlos en Docker a un nivel básico.
