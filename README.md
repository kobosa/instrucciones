# Pasos de configuración

1. Como requerimientos iniciales es necesario tener instalado:
```bash
Python 2.7
Python-pip
Docker Compose
LiClipse
```
2. Luego clonar los repositorios 
- [Kobo Docker](https://github.com/kobosa/kobo-docker)
- [Kpi](https://github.com/kobosa/kpi)
- [Enketo Express](https://github.com/kobosa/enketo-express) 

## Kobo-Docker

1. Editar el archivo **envfile.local.txt**, cambiando lo siguiente:

```bash
# IP local
HOST_ADDRESS=
# Se genera con un comando que se encuentra en el mismo archivo.
DJANGO_SECRET_KEY=
# Nombre de usuario.
KOBO_SUPERUSER_USERNAME=
# Contraseña.
KOBO_SUPERUSER_PASSWORD=
# Correo de contacto.
KOBO_SUPPORT_EMAIL=
```

2. Editar el archivo **./envfiles/kpi.txt**, cambiando lo siguiente:


```bash
KPI_PATH_FROM_ECLIPSE_TO_PYTHON_PAIRS=
localización de kpi -> /srv/src/kpi |
localización de formpack -> /usr/local/lib/python2.7/dist-packages/formpack
```


3. Editar el archivo **./docker-compose.local.yml**, cambiando lo siguiente:

```bash
kpi:
# Dev: Use live source directories from the host machine.
    - Localización directorio kpi:/srv/src/kpi
# Dev: Share PyDev remote debugger source into container.
    - Localización LiClipse Pydev Pluggin:/srv/pydev_orig:ro

enketo_express:
# Dev: Use the live `enketo-express` directory from the host machine.
    - Localización directorio enketo-express:/srv/src/enketo_express
```

## KPI
1. Instalar dependencias, ver pasos en el  [repositorio kpi](https://github.com/kobosa/kpi).
2. Abrir el proyecto en LiClipse y consigurarlo como un proyecto de Django (Referenciar el archivo **manage.py** y **settings.py**).
3. **Opcional:** En LiClipse correr el proyecto como debug.

## Enketo Express

1. Install JS prerequisites: [Node.js](https://github.com/nodesource/distributions) (8.x LTS), [Grunt Client](http://gruntjs.com).
2. Install [Redis](http://redis.io/topics/quickstart)
3. Install build-essential, curl and git with `(sudo) apt-get install build-essential git curl`
4. Clone this repository
5. Install dependencies with `npm install` from the project root
6. Create config/config.json to override values in the [default config](./config/default-config.json). Start with `cp config/default-config.json config/config.json`
7. Build with `grunt` from the project root


## Ejecutar el proyecto

Dentro del directorio raíz de kobo-docker ejecutar:
```bash
ln -s docker-compose.local.yml docker-compose.yml
docker-compose pull
docker-compose up
```


# Ejecutar con kobo-install
Una vez editado el código en **kpi**, debemos eliminar la carpeta **staticfiles** para que se compilen nuevamentos los archivos **jsapp**.
Para que se compilen los archivos de la carpeta **jsapp** se utilizara **kobo-docker** del repositorio [kobosa](https://github.com/kobosa) y correrlo con las instrucciones.

Luego eliminamos la carpeta **kobo-docker**, y proceder con los pasos:
  1. Descargar el repositorio [kobo-install](https://github.com/kobotoolbox/kobo-install)
  2. Ejecutar ```python run.py ``` 
  3. Detenemos ```python run.py --stop ```
  4. Agregamos las lineas:
   - En docker-compose.frontend.yml -> kpi -> volumenes
   ```- ../kpi:/srv/src/kpi```
- En docker-compose.frontend.yml -> enketo_express -> volumenes
```- ../enketo-express:/srv/src/enketo_express```

  5. Ejecutamos nuevamente para ver los cambios ```python run.py ``` 


# Abrir de KoboReports

  - Abrir, comando:
  ```screen -R```
   
  - Cerrar, teclas:
  Ctrl + A y luego presionamos D
