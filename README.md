<img src="https://i.ibb.co/VM5MzBT/craftech-logo3.png=150x" width="250" height="250">

## Prueba 1 - Diagrama de Red

<img src="/img/imagen.jpeg">

En el diagrama anteriormente adjuntado, tenemos una aplicacion web, deployada en AWS cloud. En la misma, utilizamos el servicio EKS que nos proporciona clústeres seguros y de alta disponibilidad y nos brinda la posibilidad de automatizar tareas clave como el aprovisionamiento de nodos y las actualizaciones.

Dentro de este cluster y para organizar mejor las tareas, podemos crear Namespaces , estos son clusteres virtuales dentro del cluster de kubernetes.  Dentro de cada uno de los namespaces vamos a encontrar los pods (me faltaron en la imagen). Los Pods son las unidades de computación desplegables más pequeñas que se pueden crear y gestionar en Kubernetes. Un pod puede tener uno o mas contenedores pero lo mejor es utilizar solo uno por pod. Alli vamos a tener nuestra aplicacion, con un pod para el frontend, uno para el backend, uno para la base de datos, etc.

Utilizamos ademas un balanceador de cargas, cuando el trafico entrante es demasiado para un solo contenedor, voy a necesitar que varios contenedores atiendan a los usuarios de manera distribuida, entonces, este balanceador de cargas es el que decide donde enviar el trafico.

Del otro lado, se observa el equipo de desarrolladores que pueden modificar las imagenes de los contenedores que se estan corriendo. 
Estas imagenes se encuentran en una plataforma llamada DockerHub donde podemos tener nuestros repositorios de imágenes, conocido como registro (Registry). 
Todo esto puede ser automatizado con algunas herramientas a traves de un workflow o pipeline. Github tiene la herramienta github actions para realizar este tipo de tareas.


## Prueba 2 - Despliegue de una aplicación Django y React.js

Para este punto, lo primero que se realizo fue hacer los Dockerfiles. Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones necesarias para crear una imagen que, posteriormente, se convertirá en una sola aplicación utilizada para un determinado propósito. Se realizo un Dockerfile para el frontend y uno para el backend ya que se quiere correr contenedores diferentes para cada parte de software. 
Entonces si se quiere correr estos contenedores debemos hacerlo con el siguiente comando:

Nos paramos en la carpeta donde esta el dockerfile y en la terminal escribimos esto
```bash
$ docker build -t <app-name>:<tag> .
```
Donde *app-name* es el nombre que se le desee dar a la aplicación y *tag* la versión de la misma.

Para construir la imagen simplemente corremos el siguiente comando:
```bash
$ docker run <app-name>
```
Para correr varios contenedores a la vez, utilizamos el archivo *docker-compose.yaml* donde escribimos los distintos servicios que queremos levantar y otras features como la red a la  que pertenecen o la vinculacion de puertos.
Para lanzar todos los servicios simplemente corremos el siguiente comando (dentro del directorio donde se encuentra el docker-compose.yaml):
```bash
$ docker-compose up
```
Para realizar el deployment, lo primero que se instalo fue Minikube. Minikube es una herramienta que administra maquinas virtuales en donde corre un cluster o mejor dicho una instancia de Kubernetes en un solo nodo de forma local.

Luego se realizaron los manifiestos de kubernetes con la herramienta *kompose*. Esta herramienta toma un archivo docker-compose y lo traduce a recursos de kubernetes como services y deployments.
Esto se realiza con el siguiente comando:
```bash
$ kompose convert -o ./<nombre-carpeta>
```
Y se guardan en la carpeta anteriormente creada con su nombre-carpeta.

Estos manifiestos seran aplicados donde corresponda dentro del cluster. En este caso se creo un namespace con el comando 

```bash
$ kubectl create namespace <nombre-namespace>
```
y alli dentro se hizo el apply.
Luego de realizar esto, se pueden observar los distintos deployments y services con los comandos:
```bash
$ kubectl get deployments
$ kubectl get services
```
Para corroborar si se han levantado bien las partes de software. Una vez que estan levantados los contenedores, podemos ingresar a un navegador web y buscar en los puertos que hemos definido y vinculado anteriormente en el docker-compose:

<img src="/img/imagen2.jpeg">

<img src="/img/imagen3.jpeg">


## Prueba 3 - CI/CD


Para la realizacion de esta parte, se utilizo la herramienta Github Actions. Es una herramienta sencilla para automatizar los flujos de trabajo (workflows) o pipelines. En este caso, se necesitaba actualizar una imagen en DockerHub cada vez que se modificaba el archivo *index.html*. Las actions se escriben en un archivo llamado *actions.yml* dentro de la carpeta .github/workflows en el siguiente repositorio:

[Enlace al repositorio](https://github.com/macagauna117/devops-interview-workflow)

