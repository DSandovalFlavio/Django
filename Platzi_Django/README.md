# Django

Características

- Open Source
- Rápido
- Seguro
- Escalable

## Seguridad en Django

Protección contra la falsificación de solicitudes en sitios cruzados (CSRF):

- Los ataques CSRF permiten que un usuario malintencionado ejecute acciones utilizando las credenciales de otro usuario sin el conocimiento de este. 👿
- Django tiene protección incorporada contra la mayoría de los tipos de ataques CSRF. 😎

Protección de inyección SQL:

- La inyección SQL es un tipo de ataque en el que un usuario malicioso es capaz de ejecutar código SQL arbitrario en una base de datos. 💉
- Los conjuntos de consultas de Django están protegidos de la inyección de SQL ya que sus consultas se construyen utilizando la parametrización de consultas.

Protección contra el clickjacking:

- El clickjacking es un tipo de ataque en el que un sitio malicioso envuelve a otro sitio en un marco. Este ataque puede hacer que un usuario desprevenido sea engañado para que realice acciones no deseadas en el sitio objetivo.

Protección contra clics en forma de X-Frame-Options middleware que en un navegador compatible puede evitar que un sitio se represente dentro de un marco.

Protección de la escritura de sitios cruzados (XSS):

Los ataques XSS permiten a un usuario inyectar scripts del lado del cliente en los navegadores de otros usuarios. 💉

El uso de plantillas Django te protege contra la mayoría de los ataques XSS.

## [Instalacion Django](Instalcion.md)

---

# Estructura de un proyecto Django

![Estructura](img/Best-Practice-to-Structure-Django-Project-Directories-and-Files.png)

Archivos y su función:

- `manage.py`: Es el punto de entrada para la administración de un proyecto Django. Con él puedes interactuar con el proyecto de varias formas.
- `settings.py`: Contiene la configuración del proyecto Django.
- `urls.py`: Contiene las definiciones de URL del proyecto Django.
- `wsgi.py`: Es el punto de entrada para servir el proyecto en producción con un servidor web compatible con WSGI.
- `asgi.py`: Es el punto de entrada para servir el proyecto en producción con un servidor web compatible con ASGI.
- `__init__.py`: Es un archivo vacío que indica a Python que este directorio debe tratarse como un paquete Python.

# Servidor de desarrollo

Para iniciar el servidor de desarrollo, ejecuta el siguiente comando:

```bash
python manage.py runserver
```

El servidor de desarrollo se ejecuta en el puerto 8000 por defecto.

# Como trabajamos en Django

En Django, podemos trabajar con dos objetos principales:

- Proyectos: Es una colección de configuraciones y aplicaciones para un sitio web particular. Un proyecto puede contener múltiples aplicaciones. Un proyecto puede ser considerado como una instancia de Django.
- Aplicaciones: Es una aplicación web que realiza una tarea específica. Puede ser una aplicación de blog, una aplicación de encuestas o cualquier otra cosa. Puede contener una variedad de archivos que definen su comportamiento.

## Crear una aplicación

Para crear una aplicación, ejecuta el siguiente comando:

```bash
python manage.py startapp <nombre de la aplicación>
```

Con esto, se creará una carpeta con el nombre de la aplicación que contiene los archivos necesarios para crear una aplicación.

Vamos a crear una nueva vista para nuestra aplicacion, para ello dentro de la carpeta de nuestra aplicacion creamos un archivo llamado `views.py` y dentro de este archivo creamos una funcion que nos devuelva un texto.

```python
# HttResponse es una funcion que nos permite devolver un texto plano
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hola mundo")
```

Ahora vamos a crear una ruta para nuestra vista, para ello vamos a editar el archivo `urls.py` de nuestra aplicacion y le agregamos la siguiente linea:

```python
# path es una funcion que nos permite crear rutas
from django.urls import path
# '.' significa que estamos importando desde el mismo directorio
# views es el archivo que contiene la funcion index
from . import views

urlpatterns = [
    # path('ruta', funcion, nombre de la ruta)
    path('', views.index, name='index'),
]
```

Ahora vamos a crear una ruta para nuestra aplicacion, para ello vamos a editar el archivo `urls.py` de nuestro proyecto y le agregamos la siguiente linea:

```python
# path es una funcion que nos permite crear rutas
# include es una funcion que nos permite incluir todas las rutas de una aplicacion
from django.urls import path, include

urlpatterns = [
    # path('ruta', funcion, nombre de la ruta)
    path('app/', include('app.urls')),
]
```

Con esto ya podemos acceder a nuestra vista desde la ruta `http://127.0.0.1:8000/polls/`
ahora ya podemos visualizar nuestro texto en el navegador.

## Que es ORM

Es un mapeo de objetos relacional, es decir, es una forma de interactuar con una base de datos relacional, pero en lugar de utilizar SQL, utilizamos objetos.

Donde la base de datos es el archivo .py que crea los modelos, y las tablas son las clases que creamos en el archivo .py

Base de datos <---> ORM <---> Clases

Tabla <---> Clase <---> Objeto

Columna <---> Atributo <---> Propiedad

## Crear un modelo de base de datos

El modelo de base de datos es la definición de la estructura de la base de datos, es decir, es la definición de las tablas y sus columnas.

Vamos a crear un modelo de 2 tablas, una llamada questions y otra llamada choices

- questions
  - id
  - question_text
  - pub_date

- choices
  - id
  - question_id
  - choice_text
  - votes

Para crear un modelo de base de datos, vamos a editar el archivo `models.py` de nuestra aplicacion y le agregamos la siguiente linea:

```python
# models es una clase que nos permite crear modelos de base de datos
from django.db import models

class Question(models.Model):
    # Id is created automatically by Django
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    # Id is created automatically by Django
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

```

## Crear una migracion

Una migracion es un archivo que contiene las instrucciones para crear las tablas en la base de datos.

Para crear una migracion, ejecuta el siguiente comando:

```bash
python manage.py makemigrations
```

Con esto, se creará una carpeta llamada `migrations` en la carpeta de nuestra aplicacion, y dentro de esta carpeta se creará un archivo llamado `0001_initial.py` que contiene las instrucciones para crear las tablas en la base de datos.

## Aplicar una migracion

Para aplicar una migracion, ejecuta el siguiente comando:

```bash
python manage.py migrate
```

Con esto, se creará las tablas en la base de datos.

## Consola interactiva de Django

Para acceder a la consola interactiva de Django, ejecuta el siguiente comando:

```bash
python manage.py shell
```

Esta consola nos permite interactuar con la base de datos, es decir, podemos crear, editar, eliminar y consultar registros de las tablas.

```python
# Importamos el modelo de la tabla
from polls.models import Question, Choice

# Creamos un registro
q = Question(question_text="¿Cual es tu color favorito?", pub_date=timezone.now())
q.save()

# Visualizamos los valores de los atributos
q.question_text
q.pub_date

# Obtenemos el id del registro
q.id

Obtenemos todos los registros de la tabla
Question.objects.all()

```

# Filtrar registros

```python
# Obtenemos todos los registros de la tabla
Question.objects.all()

# Obtenemos el primer registro de la tabla
Question.objects.first()

# Obtenemos el ultimo registro de la tabla
Question.objects.last()

# Obtenemos el registro con el id 1
Question.objects.get(id=1)

```