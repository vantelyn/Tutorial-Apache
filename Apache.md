# Documentación de Apache
*Ernest Valentin Faulhaber*
## Resumen

Proyecto dentro de la asignatura de Despliegue de Aplicaciones Web de 2º DAW en el que documentamos paso a paso el proceso de instalación y configuración de Apache en una máquina Ubuntu. Mediante este trabajo vamos a aprender la instrucciones de instalación de Apache, sus comandos de administración, y por último cómo crear y administrar nuestra página Web de tal manera que esta sea accesible a través de un dominio.
&nbsp;
## Palabras clave
* Dirección Web
* URL
* Cliente
* Servidor Web
* HTTP
* Apache
* Código abierto
* Ubuntu
* Linux
&nbsp;
## Tabla de contenidos
* **Introducción a Appache**
    * Contexto
    * Motivación
* **Instalar Apache en Ubuntu**
* **Administración de Apache**
    * Consultar el estado del servidor
    * Activar/Desactivar inicio automático
    * Inicio y Apagado del servidor
* **Administración de la Web**
    * La página Web por defecto
    * Crear nuestra propia página Web
    * Configurar el VirtualHost de nuestra Web
    * Activación del VirtualHost
    * Prueba de Navegación
* **Bibliografía**
&nbsp;
## Introducción a Appache  
### Contexto
Este proyecto sirve como documentación de la instalación de Apache en una máquina Ubuntu (Linux) dentro de la asignatura de Despliegue de Aplicaciones Web de 2º DAW, Ágil Centros.
Para llevar a cabo la instalación vamos a necesitar:
* Equipo con Ubuntu instalado (Versión 16.04 LTS o superior)
* Conocimientos básicos de líneas de comandos Linux
* Tener instalado el editor de textos gedit (opcional)
### Motivación
Los objetivos de este proyecto son:
* Aprender a instalar y configurar Apache desde cero en el entorno de Linux
* Aprender a utilizar sus opciones básicas de configuración
* Tener unos apuntes de repaso fáciles de seguir a la hora de reutilizar Apache en el futuro
&nbsp;
## Instalar Apache en Ubuntu
Para asegurarnos de que vamos a instalar la última versión disponible de Apache necesitamos primero realizar una **actualización de la librería de paquetes** de Ubuntu. Para ello vamos a ejecutar la siguiente petición en el terminal de instrucciones:
```
sudo apt update
```
Una vez terminado el proceso de actualización procedemos a la **instalación de Apache**:
```
sudo apt install apache2
```
Por último, **comprobamos que la instalación se ha compeltado con éxito** navegando desde nuestro **explorador Web** a la dirección **`127.0.0.1`** (o simplemente **localhost**) :  

![Prueba de Instalación](https://github.com/vantelyn/WebApache/blob/main/localhost.jpg?raw=true)
&nbsp;
## Administración de Apache
### Consultar el estado del servidor
Una vez instalado, el servidor Apache se va a iniciar de forma predeterminada. Para **consultar el estado** de nuestro servidor podemos utilizar la siguiente instrucción:
```
sudo systemctl status apache2
```
El resultado debería ser el siguiente:
```
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2020-04-23 22:36:30 UTC; 20h ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 29435 (apache2)
      Tasks: 55 (limit: 1137)
     Memory: 8.0M
     CGroup: /system.slice/apache2.service
             ├─29435 /usr/sbin/apache2 -k start
             ├─29437 /usr/sbin/apache2 -k start
             └─29438 /usr/sbin/apache2 -k start
```
### Activar/Desactivar inicio automático
Por defecto, Apache está configurado para iniciarse de forma automática con el inicio del sistema. Podemos **desactivar** este comportamiento con la siguiente instrucción:
```
sudo systemctl disable apache2
```
Y para volver a **activar** el inicio automático:
```
sudo systemctl enable apache2
```
Cabe destacar que **estas instrucciones no tienen ningún efecto sobre el estado actual del servidor**.
### Inicio y Apagado del servidor
Para **apagar** el servidor web podemos utilizar la siguiente instrucción:
```
sudo systemctl stop apache2
```
Y para volver a **iniciar**:
```
sudo systemctl start apache2
```
También podemos **reiniciar** el servidor mediante la instrucción:
```
sudo systemctl restart apache2
```
Incluso tenemos la opción de **recargar** el servidor para aplicar cambios de configuración sin interrumpir su funcionamiento:
```
sudo systemctl reload apache2
```
&nbsp;
## Administración de la Web
### La página Web por defecto
Apache incluye por defecto una **página web de demonstración**, la misma que hemos visualizado al finalizar el proceso de instalación:
&nbsp;
Los contenidos de esta página se ubican en la carpeta **`/var/www/html`** y podemos acceder a ellos mediante la siguiente instrucción:
```
cd /var/www/html
```
Dentro de esta carpeta encontraremos el archivo **`index.html`** cuyos contenidos podemos visualizar o modificar mediante la instrucción:
```
sudo gedit index.html
```
Cuyo contenido simplificado sería:
```
<html>
<head>
  ...
</head>
<body>
  ...
</body>
</html>
```
También tenemos la posibilidad de **modificar la configuración de la Web por defecto**:
&nbsp;
Los archivos de configuración de todas las páginas web de Apache se ubican en la carpeta **`/etc/apache2/sites-available/`** y podemos acceder a ella mediante la instrucción:
```
cd /etc/apache2/sites-available
```
El archivo de configuración de la web por defecto es el **`000-default.conf`** y podemos visualizarlo o modificarlo mediante la instrucción:
```
sudo gedit 000-default.conf
```
Cuyos contenidos simplificados serían:
```
    # 127.0.0.1 es la dirección IP del servidor y :80 el puerto de acceso
<VirtualHost 127.0.0.1:80>                      
    # ServerName especifica el nombre de dominio raiz de nuestra página web
    ServerName www.example.com           
    # ServerAlias permite introducir nombres de dominio adicionales
    ServerAlias example.com    
    # DocumentRoot especifica la ubicación de la Web dentro del servidor
    DocumentRoot "/var/www/html"      
    # ServerAdmin indica la dirección de correco electrónico del administrador del servidor
    ServerAdmin webmaster@host.example.com      
</VirtualHost>
```
### Crear nuestra propia página Web
El primer paso es **crear una carepta para nuestra página Web** en la ubicación **`/var/www/`** mediante la instrucción:
```
sudo mkdir /var/www/miweb/
```
A continuación **nos desplazamos a la carpeta** que acabamos de crear:
```
cd /var/www/miweb/
```
Y aquí **creamos el índice de nuestra Web `index.html`** mediante:
```
sudo gedit index.html
```
Cuyos contenidos los editamos siguiendo la estructura básica de un archivo HTML:
```
<html>
<head>
  <title> Título de nuestra página </title>
</head>
<body>
  <p> Hola Mundo Cruel! </p>
</body>
</html>
```
### Configurar el VirtualHost de nuestra Web
Una vez creada la página es necesario crear el archivo VirtualHost que module el acceso a nuestra Web.
&nbsp;
Para ello tenemos que acceder de nuevo a la carpeta de los archivos de configuración de las páginas Web de apache **`/etc/apache2/sites-available/`** mediante:
```
cd /etc/apache2/sites-available/
```
Dentro de esta carpeta vamos a crear nuestro propio archivo de configuración **`miweb.conf`**:
```
sudo gedit miweb.conf
```
Cuyo contenido editaremos siguiendo la estructura básica del archivo **`000-default.conf`**:
```
    # especificamos la dirección Web de nuestro servidor y el puerto de acceso
<VirtualHost 127.0.0.1:80>    
    # introducimos la dirección Web de nuestro dominio
    ServerName www.miweb.com         
    # y la ubicación de la Web dentro del servidor
    DocumentRoot /var/www/miweb    
    # junto con nuestra dirección de correo electrónico
    ServerAdmin ejemplo@agilcentros.com         
</VirtualHost>
```
### Activación del VirtualHost
Dado que de momento no disponemos de un dominio público, vamos a indicarle a nuestro sistema operativo que el dominio utilizado para nuestra página Web será resuelto por nuestra propia máquina. Para ello vamos a editar el archivo **`hosts`**, el cual se ubica dentro de la carpeta **`etc`**, mediante la instrucción:
```
sudo gedit /etc/hosts
```
Una vez abierto el archivo le añadimos la siguiente linea:
```
127.0.0.1 www.miweb.com
```
El siguiente paso sería activar el archivo de configuración de la página Web mediante la instrucción:
```
sudo a2ensite miweb.conf
```
Y por último debemos recargar nuestro servidor para aplicar los cambios introducidos:
```
sudo systemctl reload apache2
```
### Prueba de Navegación
Para comprobar si nuestra página web es accesible solo nos queda introducir el nombre de dominio elegido **`www.miweb.com`** en nuestro explorador:  

![Prueba de Configuración](https://github.com/vantelyn/WebApache/blob/main/miweb.jpg?raw=true)
Y con este paso podemos dar por concluída la configuración de nuestro servidor.
&nbsp;
## Bibliografía
[Qué es Apache](https://cwiki.apache.org/confluence/display/HTTPD/FAQ#FAQ-WhatisApache?)  
[Manual Apache](https://httpd.apache.org/docs/2.4/es/)  
[Instalar Apache](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04-es)  
[Tutorial Instalación Apache para Ubuntu](https://ubuntu.com/tutorials/install-and-configure-apache#1-overview)  
[Qué son y cómo configurar los VirtualHosts en Apache](https://www.desarrollolibre.net/blog/apache/que-son-y-como-emplear-los-virtualhost-en-apache)  


