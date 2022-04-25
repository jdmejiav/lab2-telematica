# lab2-telematica

## Link FrontEnd.

<a href="https://lab2.jdmejiav.tk">lab2.jdmejiav.tk</a>


## FrontEnd.
Para el despligue del sitio web con certificado SSL y docker, lo que hice fue desplegar el sitio web con docker en el puerto 8080 e instalar y configurar nginx en local para que redirigiera todo el tráfico http y https a ese puerto.

En el Dockerfile  se construye el sitio a html estático y luego se despliega utilizando nginx utilizando multistage builds, esto trajo consigo un problema ya que al convertir el sitio a html estático, a la hora de realizar peticiones al Backend, las hacía de manera local, entonces fue necesario hacer una configuración extra en el archivo de configuración de nginx y definir una ruta en local que apuntara a la dirección del backend.

En el archivo del package.json, cambié la dirección del back de localhost a la de la máquina desplegada en AWS.

### Pasos

#### 1.
Necesité adquirir un dominio en Freenom, el cuál fue: <a href="https://lab2.jdmejiav.tk">lab2.jdmejiav.tk</a>.

#### 2.
Para activar el certificado.

      sudo certbot --nginx -d lab2.jdmejiav.tk
      
#### 3.
crear un archivo para la configuración del sitio con el nombre del dominio

      touch /etc/nginx/sites-available/jdmejiav.tk
      
#### 4.
Pegar el contenido que está en el archivo <a href="https://github.com/jdmejiav/lab2-telematica/blob/master/lab2.jdmejiav.tk">lab2.jdmejiav.tk</a> que contiene la configuración del sitio, en el archivo se especifica la dirección del certificado SSL, se redirecciona al puerto 8080 y se establecen los headers del proxy, además fue necesario crear una configuración especial para hacer un bind de la ruta /api/books del back para poderla consumir desde local.

#### 5.
Hay que establecer un link simbólico entre los archivos en la carpeta /etc/nginx/sited-available y los que estan en la carpeta /etc/nginx/sited-enabled

      sudo ln -s /etc/nginx/sited-available/lab2.jdmejiav.tk /etc/nginx/sited-enabled/lab2.jdmejiav.tk
      
#### 6.
Reiniciar el servicio de nginx para actualizar los cambios en la configuración.

      sudo systemctl restart nginx

## Backend.
Para desplegar el Backend, fue necesario habilitar en AWS la conexión con el Frontend en el grupo de seguridad por el puerto 5000, también fue necesario modificar el archivo .env para póner la URl de la base de datos desplegada en otra instancia EC2.

Lo desplegué utilizando docker corriendo en el puerto 5000 de la máquina virtual

## Mongo
Para desplegar la base de datos en MongoDB, utlicé una instancia EC2 en AWS diferente y en esta instalé MongoDB con la configuración estándar. Edité el grupo de seguridad de la instancia para permitir el tráfico TCP desde la instancia del backend

