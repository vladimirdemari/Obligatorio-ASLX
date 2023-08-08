# TallerJulio2023

# Descripcion del proyecto
Instalamos un servidor Bastion con Linux Rocky server con el cual admninistraremos los siguientes servidores:

- RockyServer: Tiene implementado un contenedor que aloja una aplicacion en tomcat y
un apache que cumple el rol de proxy reverso

- UbuntuServer: Instalaremos el rol mariadb y alojaremos la base de datos de la aplicacion

# Contenedor Obligatorio-ASLX

Nuestro contenedor Obligatorio-ASLX: https://github.com/vladimirdemari/Obligatorio-ASLX.git

Dispone de la siguiente estructura:

- **_ansible.cfg_** 
> archivo de configuracion de Ansible 
- **_inventario_** 
> Donde indicaremos los equipos donde aplicaremos la configuracion
- **_main.yml_**
> Es nuestro playbook principal
- **_Readme.md_**
> Informacion del proyecto
- **_requirements.yml_** 
> Donde incluimos las dependencias, roles y conexiones que requieran nuestro proyecto
- **_roles_** 
> Roles que disponen de playboooks para cada tarea

# _Roles_
- **container_tomcat:**  
- **geerlingguy.apache**
- **mariadb_debian**

El Rol **container_tomcat:** Instala podman, copia los archivos necesarios: todo.war y app.properties del bastion al anfitrion.
Luego ejecuta el modulo container.podman.podman_container el cual se encarga de generar la imagen del contenedor, mapear los archivos necesarios y el puerto para la app. e inicializar el contenedor

El Rol **geerlingguy.apache** Configura Apache, chequea los certificado definidos en el vhost, agrega la configuracion del vhost del apache, chequea que esten instalados los certificados necesarios para RedHat8 o versiones superiores

El Rol **mariadb_debian** Instala mariadb y Python 3 que es necesario para trabajar con Ansible. Realiza configuracion inicial y de seguridad. En el playbook principal creamos la base de datos, las tablas y creamos el usuario todo que utilizara la applicacion web.

# _inventario_
En esta ubicacion debemos agregar los servidores en los cuales deseamos implementar nuestras colecciones.
Es por eso que agregamos dos grupos: RedHat y Debian
Aqui agregaremos a cada servidor a su grupo correspondiente, segun su familia, anexando su respectiva direccion ip

# _ansible.cfg_
Es nuestro archivo de configuracion de Ansible donde modificamos donde modificamos nuestro pathlist: inventory=/home/sysadmin/Obligatorio-ASLX/inventario
Desde donde recogera nuestro inventario de servidores

# _main.yml_
Este archivo gestiona nuestro playbook principal.
Se encarga de ejecutar tareas generales para todos nuestros servidores, como actualizaciones de paquetes.
Tambien ejecuta tareas especificas por familia, tanto para RedHat como para Debian respectivamente.

# ***PLAYBOOKS***

**1 /Obligatorio-ASLX/main.yml** 
> ***--------------- Tareas que realiza cada modulo ----------------***
---
Tareas Generales en Servidores Linux

Update RedHat Servers

Update Debian Servers

Configuracion de Servidores RedHad

Abrir puertos en el firewall

Configuracion de Servidores Debian

> ***--------------------------------------------------------------------***


**2- /container_tomcat/main.yml** 
> ***--------------- Tareas que realiza cada modulo ----------------***

> Definir variables

> Chequear version de Apache instalada

> Configurar Apache

> ***--------------------------------------------------------------------***

**3- /geerlingguy.apache/main.yml**
> ***--------------- Tareas que realiza cada modulo ----------------***
Crear directorio para todo.war

Crear directorio para app.properties

Copiar archivo de aplicación .war al host remoto

Copiar archivo de configuración al host remoto

Instalamos podman

Crear contenedor de Tomcat

***--------------------------------------------------------------------***

**4- /mariadb_debian/main.yml**
> ***--------------- Tareas que realiza cada modulo ----------------***
Copiar archivo dump de la base de datos

Restore database

Creamos usuario todo

***--------------------------------------------------------------------***




