---
layout: post
title:  "Instalación de vzdump en CentOS 6"
date:   2014-07-02 01:35:00
---

Este es el proceso necesario para instalar vzdump (junto con vzrestore) en CentOS 6.

Lo primero que hay que hacer es satisfacer las dependencias. Para empezar, hay que instalar cstream. Puedes descargarlo de aquí: http://pkgs.repoforge.org/cstream/

Coge la última versión correspondiente a tu arquitectura y descargatelo a tu equipo. Si eres como yo y reniegas de usar la interfaz gráfica, usa wget (url del paquete). Una vez descargado, toca instalarlo. Lo podemos instalar con:

yum install (nombre_paquete)

Despues hay que instalar la libreria Simple Locking file I/O para perl. Estos son los comandos:

**wget http://dag.wieers.com/rpm/packages/perl-LockFile-Simple/perl-LockFile-Simple-0.206-1.el5.rf.noarch.rpm**

**rpm -ivh perl-LockFile-Simple-0.206-1.el5.rf.noarch.rpm**

Si vas a la pagina (http://dag.wieers.com/rpm/packages/perl-LockFile-Simple) verás que hay versiones más recientes. Yo no las he probado, pero si quieres probarlas, deberían funcionar igualmente.

Ahora toca añadir la ruta donde openvz buscará la librería. Esto varía dependiendo del sistema. Al final, lo mejor es buscar la librería en el sistema y sacar la ruta a mano. Para ello ejecutamos:

**find /usr -name Simple.pm**

Devolverá varios archivos, el que importa es el primero. En mi caso /usr/lib/perl5/vendor_perl/5.8.8/LockFile/Simple.pm

Cogemos esa ruta menos LockFile/Simple.pm y editamos nuestro fichero .bashrc. 

**vim ~/.bashrc**

Añadimos:

**export PERL5LIB=(ruta)**

En mi caso:

**export PERL5LIB=/usr/lib/perl5/vendor_perl/5.8.8/**

Guardamos y cargamos la nueva variable:

**source ~/.bashrc**

Ahora ya podemos instalar vzdump. Primero lo descargamos:

**wget http://download.openvz.org/contrib/utils/vzdump/vzdump-1.2-4.noarch.rpm**

Y después lo instalamos usando yum (así tambien comprobamos las dependencias):

**yum install http://download.openvz.org/contrib/utils/vzdump/vzdump-1.2-4.noarch.rpm**

No debería dar ningún problema. Si lo da, comprueba que tienes instalado procmail (o cualquier otro MDA). Deberías tenerlo por defecto, pero es posible que lo hayas desinstalado. Una vez terminada la instalación, ya deberías poder ejecutar vzdump y vzrestore. Si al hacerlo te muestra un error extraño, comprueba que la variable está exportada (**echo $PERL5LIB**) y que la ruta es correcta.

Eso es todo. Larga vida y prosperidad.
