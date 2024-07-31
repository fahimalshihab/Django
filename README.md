# Django

## Start a project
- Create Project : ```django-admin startproject todo```
- Run            : ``` python3 manage.py runserver```
- Migrate        : ``` python3 manage.py migrate ```
- Create app     : ``` pyhton3 manage.py startapp appname``` and add this in settings.py 
- Create Admin   : ``` python3 manage.py createsuperuser``` in python admin frame are built in.

## Create a app

- views.py :
  
  ```py
  from django.shortcuts import render
  from django.http import HttpResponse

  def index(request):
    return HttpResponse('Hello IFts world')

  ```

- urls.py :

   ```py
   from django.urls import path
   from . import views

   urlpatterns = [
    path('',views.index),
   ]

   ```


Now add the created url to the main url
- urls.py :
  
```py
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('tasks.urls')),
]
```


