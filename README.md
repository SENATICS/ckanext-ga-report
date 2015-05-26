ckanext-ga-report
=================

**Status:** In current use (datos.gov.py)

**CKAN Version:** 1.7.1+

Esta librería extiende ckanext-ga-report a modo de agregarle soporte para internacionalización del lenguaje y templates con jinja2.

**Requerimientos:** tener una cuenta creada y configurada en google analytics y tener instalada la extensión opendatagovpy.

Instalación
-----------

1. Instalar python-gflags y google-api-python-client
```
$ sudo apt-get install python-gflags
$ easy_install --upgrade python-gflags
$ pip install google-api-python-client
```

2. Activar el entorno virtual
```
$ . /usr/lib/ckan/default/bin/activate
```

3. Ir al directorio de extensiones
```
$ cd ckan/lib/default/src
```

4. Clonar el repositorio
```
$ git clone <repositorio-senatics-ckanext-ga-report>
```

5. Ingresar al directorio de la extensión e instalarla con pip
```
$ pip install -e ./
```

6. Agregar la extensión a la lista de plugins del catálogo en el archivo de configuracion (development.ini/production.ini)
```
ckan.plugins = ga-report
```

7. Agregar las configuraciones de la extensión en el archivo de configuración del catálogo (development.ini/production.ini)
```
googleanalytics.id = UA-57174147-3 (id de la propiedad de la cuenta que se quiere utilizar)
googleanalytics.account = vojeda (nombre de la cuenta de analytics)
ga-report.period = monthly
ga-report.bounce_url = /
```

8. Ingresar al directorio de la extensión opendatagovpy y ejecutar el comando de internacionalización
```
paster translations merge -l es -c /home/rparra/ckan/etc/default/development.ini
```

9. Inicializar la base de datos
```
paster initdb --config=/etc/ckan/default/development.ini
```

Autorización
------------

Antes de acceder a los datos se debe crear un token OAuth del tipo "Installed application", lo cual puede hacerse siguiendo las siguientes instrucciones:
1. Ir a "Google APIs Console" utilizando una cuenta de Google que tenga acceso a la cuenta Google Analytics de catálogo.
2. Crear un proyecto nuevo o utilizar uno existente.
3. En el panel de servicios "Services pane" activar la extensión "Analytics API" para el proyecto.
4. Ir a al panel de acceso "API Access Pane" y hacer click en "Create new Client ID" para crear un nuevo token.
5. Seleccionar el tipo "Installed application".
6. Luego de que el token haya sido creado puede ser descargado en formato json con todas las configuraciones necesarias para la extensión.

7. Descargar el token en formato json y guardarlo dentro del directorio de la extensión con el nombre credentials.json

8. Obtener el token de autenticación
```
paster getauthtoken --config=/etc/ckan/default/development.ini
```
** Observación:** Este comando abrirá una página en el navegador pidiendo permiso de acceso a los datos de la cuenta de Google analytics, para aceptar debe estar logueado como un usuario de dicha cuenta. Si este paso se realiza con éxito se creará un archivo token.dat dentro del directorio de la extensión.

9. Referenciar al archivo que acaba de ser creado desde el archivo de configuración de CKAN (development.ini/production.ini).
```
googleanalytics.token.filepath = ~/pyenv/token.dat (ubicación del token de autenticación)
```

10. Cargar las estadísticas desde la cuenta de google analytics
```
paster loadanalytics latest --config=/etc/ckan/default/development.ini
```

11. Reiniciar el servidor con el comando paster
```
paster serve /etc/ckan/default/development.ini
```

* Para verificar ir a la página de estadísticas en: http://localhost:9000/data/site-usage
