# INSTRUCTIONS

1. Create virtual envirement<br>
   1. Command: ```python3 -m venv ./venv``` in the destination folder.
   2. Activate the venv on the editor's terminal: ```venv\Scripts\bin\activate.bat```
2. Install Django
   1. ````pip install django````
   2. Check if it was installed correctly: ````pip freeze```` to view installed folders.
3. To create project, in terminal: ````django-admin startproject 'projectname'````. Starting the project some files were created, between these files we have:
   1. ````__init__.py```` - serves to determine that the 'project' folder is a package.
   2. ````settings.py```` - this file contais all the project configurations.
   3. ````urls.py```` - contains all the project's links.
   4. ````manage.py```` - to manage the server connection.

4. In ````project\settings.py```` change variable: ````TIME_ZONE = 'America/Sao_Paulo'````
5. Install Pylint plugin in Pycharm
6. To execute server, in terminal: ````python manage.py runserver````
7. To create an app, open a new terminal (because the first is running the server): ````python manage.py startapp 'nameapp'````
8. Add app to project -> Settings : 
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
9. Create ````urls.py```` inside app.
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
from django.http import HttpResponse


# Create your views here.

def index(request):
    return HttpResponse('<h1>Receitas</h1> <h2>Bem Vindo</h2>')
````
11. Add url in ````urls.py```` in project.
````
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('recipes.urls')),
]
````