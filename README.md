# lab2-telematica

## FrontEnd.
Para el despligue del sitio web con certificado SSL y docker, lo que hice fue desplegar el sitio web con docker en el puerto 8080 e instalar y configurar nginx en local para que redirigiera todo el tráfico http y https a ese puerto.

En el Dockerfile  se construye el sitio a html estático y luego se despliega utilizando nginx utilizando multistage builds, esto trajo consigo un problema ya que al convertir el sitio a html estático, a la hora de realizar peticiones al Backend, las hacía de manera local, entonces fue necesario hacer una configuración extra en el archivo de configuración de nginx y definir una ruta en local que apuntara a la dirección del backend

### Pasos

#### 1.
Necesité adquirir un dominio en Freenom, el cuál fue: <a href="https://lab2.jdmejiav.tk">lab2.jdmejiav.tk</a> 

#### 2.
Para activar el certificado.

      sudo certbot --nginx -d jdmejiav.tk
      
#### 3.
crear un archivo para la configuración del sitio con el nombre del dominio

      touch /etc/nginx/sites-available/jdmejiav.tk
      
#### 4.
Pegar el contenido que está en el archivo <a href="https://github.com/jdmejiav/lab2-telematica/blob/master/lab2.jdmejiav.tk">lab2.jdmejiav.tk</a> que contiene la configuración del sitio, en el archivo se especifica la dirección del certificado SSL, se redirecciona al puerto 8080 y se establecen los headers del proxy, además fue necesario crear una configuración especial para hacer un bind de la ruta /api/books del back para poderla consumir desde local.

#### 5.
Hay que establecer un link simbólico entre los archivos en la carpeta /etc/nginx/sited-available y los que estan en la carpeta /etc/nginx/sited-enabled

      sudo ln -s /etc/nginx/sited-available/jdmejiav.tk /etc/nginx/sited-enabled/jdmejiav.tk
      
#### 6.
Reiniciar el servicio de nginx para actualizar los cambios en la configuración.

      sudo systemctl restart nginx

## Ver resultado
