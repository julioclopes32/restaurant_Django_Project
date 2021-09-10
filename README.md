## RECIPE MANAGEMENT SYSTEM IN DJANGO

[This is the link to access the Website!](https://restaurant-django-project.herokuapp.com/)




## CRIANDO UM WEB-SITE EM DJANGO DO ZERO!

1. Create virtual envirement<br>
   1. Command: ```python3 -m venv ./venv``` in the destination folder.
   2. Activate the venv on the editor's terminal: ```venv\Scripts\bin\activate.bat```
2. Install Django
   1. ````pip install django````
   2. Check if it was installed correctly: ````pip freeze```` to view installed folders.
3. To create *project*, in terminal: ````django-admin startproject 'projectname'````. Starting the project some files were created, between these files we have:
   1. ````__init__.py```` - serves to determine that the 'project' folder is a package.
   2. ````settings.py```` - this file contais all the project configurations.
   3. ````urls.py```` - contains all the project's links.
   4. ````manage.py```` - to manage the server connection.

4. In ````project\settings.py```` change variable: ````TIME_ZONE = 'America/Sao_Paulo'````
5. Install Pylint plugin in Pycharm
6. To execute server, in terminal: ````python manage.py runserver````
7. To create an *app*, open a new terminal (because the first is running the server): ````python manage.py startapp 'nameapp'````
8. Add app to *project* -> Settings : 
````
INSTALLED_APPS = [
    'appname',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
````
9. Create ````urls.py```` inside *app*.
````
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index')
]
````
10. Add index.html to ````views.py````.
````
from django.shortcuts import render


# Create your views here.

def index(request):
    return render('request, index.html')
````
11. Add url in ````urls.py```` in *project*.
````
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('appname.urls')),
]
````
12. Create folder ````templates```` in *appfolder*, with ````index.html```` code inside.
13. Include template path in *project* .
Go to setting, specify ````DIRS```` in TEMPLATES:
````
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'appname/templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
````
14. Include static files in *project* CSS, Javascript, Images, etc.
In *project* settings go to the end, in ````#Static files````:
````
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.2/howto/static-files/

STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'project/static')
]

````
Create static folder inside *project* and put all static files.
15. In terminal type: ````python manage.py collectstatic````, to make a copy from the static files to better understanding of django.


## Manipulating project Files
1. Adjusting HTML files:
    1. At the top of the HTML, below `<DOCTYPE html>` type: `{% load static %}`, to include python code and load static folder.
    2. At all paths and src, replace `href=ref` to `href='{% static 'ref' %}`.
2. Change url references: replace `href=ref` to `href='{% url 'ref' %}`.
3. add paths to receita page ``receita.html`` in *appfolder* -> `urls.py`
4. To prevent duplicate HTML code, we can create a `base.html` with the head and body JavaScript files:
````
<!DOCTYPE html>
{% load static %}
<html lang="en">

<head>
    <meta data>
    ...
    
    <!-- Título -->
    ...

    <!-- Favicon -->
    ...

    <!-- Stylesheet -->
    ...

</head>
<body>
    {% block content %} {% endblock %}
    <!-- ##### All Javascript Files ##### -->
    ...
</body>

</html>
````
Use `{% load static %}` to import static files, and type `{% block content %} {% endblock %}` to the `<body>` tag, to the pace where the `index.html` body will be loaded. 

From the `index.html`:
````
{% extends 'base.html' %}
{% load static %}
{% block content %}

... inner body code

{% endblock %}
````

5. To prevent duplicate HTML code, inside `index.html`, like menu or footer, we can use partials. Inside templates create folder `partials`, add `footer.html` and `menu.html`.

menu.html:
````
{% load static %}
<!-- ##### Header Area Start ##### -->
    <header>
    ...
    </header>
````
footer.html:
````
{% load static %}
<!-- ##### Footer Area Start ##### -->
    <footer>
    ...
    </footer>
````

To add menu.html and footer.html inside `index.html`, we do:

index.html:
````
{% extends 'base.html' %}
{% load static %}
{% block content %}

... 
{% include 'partials/menu.html' %}
... 
{% include 'partials/footer.html' %}
...

{% endblock %}
````

6. Iterating HTML code dynamically.

Inside `index.html` we have a list of recipes displayed, instead of add code by hand, we can use an iterator to load each object html code.

to display 1 recipe we have:
````
<!-- Single Best Receipe Area -->
<div class="col-12 col-sm-6 col-lg-4">
  <div class="single-best-receipe-area mb-30">
      <img src="{% static 'img/bg-img/foto_receita.png' %}" alt="">
      <div class="receipe-content">
          <a href="{% url 'receita' %}">
              <h5>Nome da receita</h5>
          </a>
      </div>
  </div>
</div>
````
we can pass a dictionary in `views.py`.
````
def index(request):
    receitas = {
        1: 'Lasanha',
        2: 'Sopa de Legumes',
        3: 'Sorvete'
    }

    dados = {
        'nome_das_receitas': receitas
    }
    return render(request, 'index.html', dados)
````
To iterate in `index.html`, we do:
````
<div class="col-12 col-sm-6 col-lg-4">
  <div class="single-best-receipe-area mb-30">
      <img src="{% static 'img/bg-img/foto_receita.png' %}" alt="">
      <div class="receipe-content">
          <a href="{% url 'receita' %}">
              <h5>Nome da receita</h5>
          </a>
      </div>
  </div>
</div>
````
