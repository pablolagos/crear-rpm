# crear-rpm
Vamos a ver cómo crear un RPM para CentOS/RedHat/Fedora y sus variaciones en una maquina CentOS

## Preparación
Primero debemos instalar el paquete rpm-build:
````bash
yum install rpm-build rpmdevtools
````
Luego usaremos salimos de root y nos logueamos como un usuario normal sin privilegios.
Como el usuario sin privilgio crearemos la estructura del RPM en el directorio home ~

````bash
rpmdev-setuptree
````
Con este comando se ha creado la siguiente estructura:

````bash
ls -al ~/rpmbuild/
total 28
drwxrwxr-x  7 centos centos 4096 Sep 23 19:20 .
drwx------. 5 centos centos 4096 Sep 23 19:20 ..
drwxrwxr-x  2 centos centos 4096 Sep 23 19:20 BUILD
drwxrwxr-x  2 centos centos 4096 Sep 23 19:20 RPMS
drwxrwxr-x  2 centos centos 4096 Sep 23 19:20 SOURCES
drwxrwxr-x  2 centos centos 4096 Sep 23 19:20 SPECS
drwxrwxr-x  2 centos centos 4096 Sep 23 19:20 SRPMS
````
Luego ponemos los originales en la carpeta SOURCES.
Los originales pueden ser archivos comprimidos o bien archivos sueltos o una mezcla entre ambos. No hay limitaciones en cuanto a esta parte.

````bash
cd ~/rpmbuild/SOURCES
wget https://www.openssl.org/source/mi-software.tar.gz
````

Después debemos crear el archivo SPEC:

````bash
cd ~/rpmbuild/SPECS
rpmdev-newspec pxshield
````

## El archivo SPEC

## Los archivos init para SystemV y SystemD

Esta sección es muy importante, ya que todo software que deba ejecutarse en forma automática a modo de servicio, como por ejemplo httpd, sshd, entre otros, requiere un archivo init script.

Actualmente son dos las formas más utilizadas: SystemV y SystemD

SystemV es utilizado por distribuciones basadas en Redhat hasta la version RHEL 6. Esto incluye CentOS, Fedora, Red Hat y Amazon V1
SystemD es utilizado por RHEL7 en adelante, Debian 8 (Jessie), Ubuntu 15.04 y Amazon V2

## Inspeccionando un RPM

##### Listar todos los archivos de un RPM:

````bash
rpm -qlp ./path/to/test.rpm
````

Por ejemplo:

````bash
$ rpm -qlpv ./packagecloud-test-1.1-1.x86_64.rpm
-rwxr-xr-x   1   root    root    8286 Jul 16  2014 /usr/local/bin/packagecloud_hello
````

En el ejemplo, el comando se usó con el modificador **-q** para especificar que es un comando de consulta (query), **-l** para listar el contenido del paquete. El modificador **-v** u¡indica mayor verbosidad, por lo que el resultado incluye más informacion de cada archivo, como el modo, el dueño, el tamaño y la fecha.
Por último, el modificarpo **-p** indica que se trata de un paquete rpm que se encuentra en un archivo y no entre los paquetes que ya están instalados.

##### Listar los archivos de un RPM instalado

````bash
rpm -qlv openssh-server
````

##### Mostrar los script preinstall y postinstall de un rpm

````bash
$  rpm -q --scripts openssh-server
preinstall scriptlet (using /bin/sh):
getent group sshd >/dev/null || groupadd -g 74 -r sshd || :
getent passwd sshd >/dev/null || \
  useradd -c "Privilege-separated SSH" -u 74 -g sshd  -s /sbin/nologin \
  -s /sbin/nologin -r -d /var/empty/sshd sshd 2> /dev/null || :
postinstall scriptlet (using /bin/sh):
/sbin/chkconfig --add sshd
preuninstall scriptlet (using /bin/sh):
if [ "$1" = 0 ]
then
	/sbin/service sshd stop > /dev/null 2>&1 || :
	/sbin/chkconfig --del sshd
fi
postuninstall scriptlet (using /bin/sh):
/sbin/service sshd condrestart > /dev/null 2>&1 || :
````

##### Listar los archivos marcados como de configuraion de un rpm

Los archivos de configuracion no se sobreescriben al hacer update de rpm

````bash
rpm -qcv openssh-server
-rw-r--r--    1 root    root                      341 Aug 31  2017 /etc/pam.d/ssh-keycat
-rw-r--r--    1 root    root                      616 Aug 31  2017 /etc/pam.d/sshd
-rw-------    1 root    root                     3879 Aug 31  2017 /etc/ssh/sshd_config
-rw-r-----    1 root    root                      438 Aug 31  2017 /etc/sysconfig/sshd
````
