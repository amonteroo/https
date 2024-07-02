# HTTPS

## Traefik, Portainer y Docker Compose: Un equipo poderoso para crear páginas web con HTTPS

### Introducción

En el mundo del desarrollo web, la creación de sitios web seguros y accesibles es fundamental. Ahí es donde entran en juego herramientas como Traefik, Portainer y Docker Compose. Juntas, estas herramientas permiten a los desarrolladores crear y administrar páginas web con HTTPS de manera automatizada y eficiente. 
##### Nota: Se recomeinda que primero aprendan algo de docker basico para poder entender mejor de como funciona los contenedores y su puesta en produccion porque si tienen errores deben saber como encontralos

### ¿Qué es Traefik?

Traefik es un proxy inverso y balanceador de carga moderno que facilita la exposición de servicios en una red. Actúa como intermediario entre los usuarios y los servicios, enrutando el tráfico a los contenedores o aplicaciones correctas. Traefik también puede generar automáticamente certificados TLS/SSL, lo que garantiza que sus sitios web sean seguros y accesibles a través de HTTPS.

### ¿Qué es Portainer?

Portainer es una interfaz de usuario web que facilita la administración de contenedores Docker. Proporciona una interfaz gráfica intuitiva para realizar tareas comunes como iniciar, detener, escalar y depurar contenedores. Portainer también se puede integrar con Traefik para simplificar aún más la administración de sus servicios web.

### ¿Qué es Docker Compose?

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones multicontenedor como una sola unidad. Simplifica la configuración y el despliegue de aplicaciones complejas que requieren múltiples contenedores. Docker Compose se puede utilizar para crear un archivo de configuración que define todos los contenedores y sus dependencias, lo que facilita la implementación y administración de su aplicación.

### Trabajando juntos

Cuando se combinan, Traefik, Portainer y Docker Compose forman una poderosa plataforma para crear y administrar páginas web con HTTPS. Docker Compose se utiliza para definir y ejecutar la aplicación, Portainer proporciona una interfaz gráfica para administrar los contenedores y Traefik se encarga de enrutar el tráfico y generar certificados SSL.

### Beneficios de usar Traefik, Portainer y Docker Compose

- Automatización: Traefik puede generar automáticamente certificados SSL y enrutar el tráfico a sus aplicaciones, 
lo que reduce la necesidad de configuración manual.
- Simplicidad: Portainer proporciona una interfaz gráfica intuitiva para administrar sus contenedores, lo que 
facilita el uso de Docker incluso para principiantes.
- Escalabilidad: Docker Compose permite escalar fácilmente sus aplicaciones agregando o eliminando contenedores 
según sea necesario.
- Seguridad: Traefik garantiza que sus sitios web sean seguros y accesibles a través de HTTPS mediante la generación
y administración de certificados SSL.

## Conclusión

Traefik, Portainer y Docker Compose son herramientas valiosas para desarrolladores web que buscan crear y administrar páginas web con HTTPS de manera eficiente y segura. Al combinar estas herramientas, puede simplificar su flujo de trabajo, mejorar la seguridad de sus sitios web y escalar fácilmente sus aplicaciones para satisfacer las demandas cambiantes.


---

## Forma basica de uso:

- Primero debe instalar docker y docker-compose en su server
- Bajarse el proyecto en la carpeta de despliegue que vaya a utilzar
###### - Debes remplazar el dominio que utilizaras, el correo para LetsEncrypt y el password para la base de datos mariadb en los archivos docker-compose.yml
- Entrar a la carpeta traefik-portainer donde eh integrago los 2 servicios en un solo docker-compose.yml
- Crear primero la red externa que utilizara el servicio Traefik
>   sudo docker network create traefik-net
- Ejecuta con el comando siguiente para crear los servicios de traefik y portainer
>   sudo docker-compose up -d
- Verifique que el dominio que haya configurado aparezca en linea [traefik.tu-dominio y portainer.tu-dominio]
- Si el punto anterior funciona correctamente entrar a la carpeta de wordpress/website/ y ejecutamos el mismo comando:
>  sudo docker-compose up -d
  ###### Esto creara un servicio de wordpress con el dominio que hayas utilizado [tu-dominio.com] y podras ver la pantalla wordpress la cual solo debes terminar de connfigurar y listo!, ya! tienes tu website con https en minutos.
- Si quieres agregar N cantidad de paginas web?, solo debes crear nuevas carpetas duplicando la primera dentro de wordpress.
  	- Te recomiendo lo hagas de forma ordenada de forma que cada carpeta se llame igual que el dominio de la
  	  pagina que crees. Por ejemplo:
  	  	* wordpress              #carpeta principal
  	  	  	- codigopage.com        #carpeta de dominio donde dentro modificaras el archivo docker-compose para el dominio deseado.
  	  	  	- micocinadelly.net
  	  	  	- tiendapetvip.shop
  	  	  	- loquedesees.online
   	



---

## Traefik nos permite activar con un par de lineas de comando restringir el acceso con user y password:

##### Primero descomentar las siguientes lineas
 - "traefik.http.routers.traefik.middlewares=authtraefik"
 - "traefik.http.middlewares.authtraefik.basicauth.users=my_user:my_passwd"

Investiga primero como se encripta basiacamente un password MD5.

Bueno!, asumo que ya saben usar docker pueden ejecutar esto:
> sudo docker run --rm httpd:2.4-alpine htpasswd -nb [USER] [password] | sed -e s/\\$/\\$\\$/g

Ejemplo: Supongamos que utilisaremos el usuario Admin y el password: G1tB3st!
> sudo docker run --rm httpd:2.4-alpine htpasswd -nb Admin G1tB3st! | sed -e s/\\$/\\$\\$/g

##### Salida: Admin:$$apr1$$3KInEDKr$$deq8JBwL.OO5eW1pvunvh1

Finalmente quedaria asi al reemplazarlo en la line de traefik:
>   "traefik.http.middlewares.authtraefik.basicauth.users=Admin:$$apr1$$3KInEDKr$$deq8JBwL.OO5eW1pvunvh1"


### Espero, esto le sirva a muchos. Good Luck...
