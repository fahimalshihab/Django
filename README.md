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

## create database

- models.py :
```py
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=200)
    complete = models.BooleanField(default=False)
    created =  models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```
## add in html
```
<h3>To Do</h3>

{% for task in tasks %}
    <div>
        <p>{{task}}</p>

    </div>

{% endfor %}

```

## register the model in admin.py

- admin.py :
```py
from django.contrib import admin

from .models import *

admin.site.register(Task)
```

## add in views.py

-views.py :
```py
from django.shortcuts import render
from django.http import HttpResponse

from .models import *

def index(request):

    tasks = Task.objects.all()
    context = {'tasks':tasks}
    return render(request,'tasks/list.html',context)
```

## Create Form

- forms.py :
```py
from django import forms
from django.forms import ModelForm

from .models import *

class TaskForm(forms.ModelForm):

    class Meta:
        model = Task
        fields = '__all__'
```
add in views.py
- views.py :
```py
from django.shortcuts import render
from django.http import HttpResponse

from .models import *
from .forms import *

def index(request):

    tasks = Task.objects.all()
    form = TaskForm()
    context = {'tasks':tasks,'form':form}
    return render(request,'tasks/list.html',context)
```
add this in list.html
- list.html:
```
<h3>To Do</h3>
<form>
    {{form.title}}
    <input type="submit" name = "Create Task">
</form>

{% for task in tasks %}
    <div>
        <p>{{task}}</p>

    </div>

{% endfor %}
```
## Create  items
- list.html :
```
<h3>To Do</h3>
<form method="POST" action="/">
    {%csrf_token%}
    {{form.title}}
    <input type="submit" name = "Create Task">
</form>

{% for task in tasks %}
    <div>
        <p>{{task}}</p>

    </div>

{% endfor %}
```
- views.py :
```py
from django.shortcuts import render,redirect
from django.http import HttpResponse

from .models import *
from .forms import *

def index(request):

    tasks = Task.objects.all()
    form = TaskForm()
    if request.method == 'POST':
        form = TaskForm(request.POST)
        if form.is_valid():
            form.save()
        return redirect('/')


    context = {'tasks':tasks,'form':form}
    return render(request,'tasks/list.html',context)
```

