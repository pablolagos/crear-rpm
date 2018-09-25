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

En el ejemplo, el comando se usó con el modificador **-q** para especificar que es un comando de consulta (query), **-l** para listar el contenido del paquete. El modificado **-v** u¡indica mayor verbosidad, por lo que el resultado incluye más informacion de cada archivo, como el modo, el dueño, el tamañoy la fecha.

