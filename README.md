# Odoo con Docker Compose

Este repositorio contiene los archivos necesarios para configurar Odoo utilizando Docker Compose.


## Índice

1. [Estructura del Repositorio](#estructura-del-repositorio)
2. [Configuración del Docker Compose](#configuración-del-docker-compose)
    - [Odoo](#odoo)
    - [Servicio de la base de datos PostgresSQL](#servicio-de-la-base-de-datos-postgressql)
3. [Comandos para Ejecutar los Contenedores](#comandos-para-ejecutar-los-contenedores)
    - [Para iniciar los contenedores:](#para-iniciar-los-contenedores)
    - [Para detener los contenedores:](#para-detener-los-contenedores)
4. [Enlace con PyCharm y la Base de Datos](#enlace-con-pycharm-y-la-base-de-datos)
5. [Funcionalidades Relevantes con Python en Odoo](#funcionalidades-relevantes-con-python-en-odoo)
6. [Resolución de Conflictos en el Puerto 5432](#resolución-de-conflictos-en-el-puerto-5432)
7. [Proceso al ejecutar el Odoo](#proceso-al-ejecutar-el-odoo)

## Estructura del Repositorio

- `docker-compose.yml`: Archivo principal de Docker Compose que especifica la configuración de los contenedores.
- `odoo.conf`: Archivo de configuración de Odoo.
- `README.md`: Este archivo, que proporciona información sobre el repositorio y su configuración.
- `postgres_data/`: Directorio para almacenar los datos persistentes de PostgreSQL.

## Configuración del Docker Compose

El archivo `docker-compose.yml` especifica los servicios necesarios para ejecutar Odoo y PostgreSQL, junto con las 
configuraciones y enlaces necesarios.

### Odoo
```yaml

version: '3.1'
services:
  # servicio de la aplicación python
  web:
    # imagen de la aplicación, con su version
    image: odoo:16.0
    # servicio de base de datos que utiliza la aplicación
    # esta aplicacion depende deste servicio
    # no arrancara hasta que el servicio este levantado
    depends_on:
      - mydb
    # mapeo de puertos para poder acceder desde la máquina
    ports:
      - "8069:8069"
    # varibles de entorno
    environment:
      # el nombre o la ip del gestor de base de datos
      - HOST=mydb
      # usuario admnistrador/ superusuario del gestor de la base de datos
      - USER=odoo
      # contraseña del admiistrador del gestor de base de datos
      - PASSWORD=odoo
```
### Servicio de la base de datos PostgresSQL
```yaml
  # servicio gestor de base de datos
  mydb:
    # imagen utilizada y su version
    image: postgres
    # mapeo de puertos para poder acceder a las bases de datos desde la IDE
    ports:
      - "5432:5432"
    # variables de entorno de la imagen postgres:15
    environment:
      # usuario administrador
      #no podemos cambiar porque odoo por defecto se conecta como "postgres
      - POSTGRES_USER=odoo
      # contraseña superuser para conectarse desde fuera
      - POSTGRES_PASSWORD=odoo

      - POSTGRES_DB=postgres

```
- Para esto también desde nuestro propio IDE, crearemos una Database como en la imagen, primero le daremos al `+` luego 
seleccionaremos la base de datos en cuestión, en mi caso PostgresSQL.

![](imagenes/Cap_BD_PostgresOdoo.png)

- Ahora seguiremos con la configuración de misma, pondremos lo que tenemos en el `docker-compose.yml` como el puerto, 
el user y la contraseña que pusimos en el servicio.

![](imagenes/BD_Conf_Odoo.png)


## Comandos para Ejecutar los Contenedores
En la terminal de tu propia IDE los siguientes comandos, importante si estas en portatil tener el Docker 
Desktop iniciado, también puedes iniciarlo y pararlo desde ahí.
### Para iniciar los contenedores:
```bash
docker-compose up -d
``` 
### Para detener los contenedores:
```bash
docker-compose down
```
## Enlace con PyCharm y la Base de Datos
PyCharm se puede enlazar con el contenedor de Docker utilizando la configuración proporcionada en el archivo
`docker-compose.yml`. Para acceder a la base de datos desde PyCharm, utiliza las credenciales proporcionadas en el archivo
`docker-compose.yml` y el puerto 5432.


## Funcionalidades Relevantes con Python en Odoo
Python es fundamental en Odoo para:

1. Desarrollo de Módulos: Permite crear módulos personalizados para agregar, modificar o adaptar funcionalidades según 
las necesidades de la empresa.

2. Interacción con el Usuario: Controla la lógica empresarial y la interacción con el usuario, creando interfaces 
amigables y dinámicas.

3. Automatización de Procesos: Facilita la automatización de tareas, como la generación de informes, la programación de 
tareas periódicas y la integración con otros sistemas.

4. Integración con Herramientas Externas: Posibilita la integración de Odoo con sistemas externos mediante el uso de 
API y bibliotecas específicas.

5. Personalización de Informes y Documentos: Permite personalizar y generar informes y documentos, incluyendo la 
creación de plantillas y la automatización del proceso.

6. Optimización del Rendimiento: Se utiliza para optimizar el rendimiento del sistema mediante la mejora del código,
consultas a la base de datos y técnicas de escalabilidad.

## Resolución de Conflictos en el Puerto 5432
Si el puerto 5432 está ocupado en el ordenador local, puedes solucionarlo modificando el puerto en el archivo
`docker-compose.yml` en la sección del servicio de la base de datos. Por ejemplo, cambia "5432:5432" a "5433:5432" para 
mapear el puerto local 5433 al puerto 5432 del contenedor. Luego, asegúrate de ajustar las configuraciones de PyCharm y 
cualquier otra aplicación que necesite acceder a la base de datos en consecuencia.

## Proceso al ejecutar el Odoo
Para ver el odoo tendremos que ir a la url `localhost:8069` y nos aparecera la pagina de inicio de sesion de odoo.

![](imagenes/Cap_inicio_Odoo.png)

Para tener en cuenta lo que debes hacer es cambiar la `Database Name` a una que no sea postgres que te dara error lo
recomendable seria mysql por ejemplo. Y la `Password` la que pusiste en el `docker-compose.yml`.