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
Con este comenado se ha creado la siguiente estructura:

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



