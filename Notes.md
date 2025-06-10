# Django Web Notes

## What is Django?

$$\begin{aligned}
\cdot \quad
&\text{Django is a high-level Python web framework that enables rapid development of secure and maintainable websites.} \\
\cdot \quad
&\text{It’s free and open-source.} \\
\cdot \quad
&\text{Django emphasizes reusability, less code, and the DRY (Don’t Repeat Yourself) principle.}
\end{aligned}
$$

---

## How to Install Django

```bash
pip install django
```

---

## Starting a Project in Dajango

```bash
django-admin startproject project_name
```

### Project structure

After hitting above command following structre will created:

```md
project_name/
    manage.py
    project_name/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

---

## Files Definition in Django project Files Folder

### 1. init File

```python
__init__.py
```

$\begin{aligned}
    &\text{This file is used by django framework to read files and folders of the project folder.}
\end{aligned}$

### 2. `asgi.py`

$\begin{aligned}
    &\text{This is a server file, abbrevated as asyncrhonus gateway interface.}
\end{aligned}$

### 3. `wsgi.py`

$\begin{aligned}
    &\text{Abbrevated as web server gateway interface.}
\end{aligned}$

### 4. `urls.py`

$\begin{aligned}
    &\text{This file is used for mapping all urls used in the project (means website url like homepage url and etc).}
\end{aligned}$

### 5. `settings.py`

$\begin{aligned}
    &\text{This file is used for setting of all settings of project i.e. adding installed apps etc.}
\end{aligned}$

### 6. `manage.py`

$\begin{aligned}
    &\text{A command-line tool for managing the project (e.g., running the server, applying migrations).}
\end{aligned}$

---

## Creating an app in Django

`Note: Go to project Directory you have created using project creation command and then run following command to create an app.`

```bash
python manage.py startapp app_name
```

`After Creating app you must have to save it in intalled apps so add following in` **project_name/settings.py**

```python
INSTALLED_APPS = [
    ...
    'app_name',  # our app name
]
```

### Folder Structre

```markdown
app_name/
├── admin.py
├── apps.py
├── __init__.py
├── migrations/
│   └── __init__.py
├── models.py
├── tests.py
└── views.py
```

---

## Files Definition in Django project app Folders

### 1. `admin.py`

$\begin{aligned}
    &\text{It registers models for Django’s admin interface.}
\end{aligned}$

### 2. `apps.py`

$\begin{aligned}
    &\text{It is configuration for the app.}
\end{aligned}$

### 3. `models.py`

$\begin{aligned}
    &\text{It define database models.}
\end{aligned}$

### 4. `views.py`

$\begin{aligned}
    &\text{It define functions/classes that handle requests and return responses.}
\end{aligned}$

### 5. `tests.py`

$\begin{aligned}
    &\text{In this file we write tests for our app.}
\end{aligned}$

### 6. `migrations/`

$\begin{aligned}
    &\text{It stores migration files for database changes.}
\end{aligned}$

---

## Views & URLs

### Views

Views are Python functions (or classes) that process requests and return responses.

Example `views.py`:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, world!")
```

### Connecting Views to URLs

To make our view accessible via a browser, we need to map it to a URL.

Create or edit `myapp/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

Then, include your app’s `urls.py` in the project’s `urls.py` (`myproject/urls.py`):

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # include your app’s URLs
]
```

`Using include() allows us to keep the project modular and organized.`

#### **Path vs re\_path**

* **`path()`**: Simple, readable syntax for defining URLs.
* **`re_path()`**: Uses regular expressions for complex patterns.

Example:

```python
# Using path
path('blog/<int:post_id>/', views.blog_detail, name='blog_detail')

# Using re_path
from django.urls import re_path
re_path(r'^blog/(?P<post_id>\d+)/$', views.blog_detail, name='blog_detail')
```

### Homepage URL

Homepage url is empty like given one

```python
path('',homepage_view_in_view_python_file)
```

---

## Creating a View

```python

def home(request): # It must has request parameter
    return HttpResponse('Hello')
```

#### OR

we can return an `.html` file

```python
def home(request):
    return render(request,'index.html')
```

---

## Creating `.html` files (Tempelates) & Adding Python Code in `.html` files

By default django will take `.html` files from templates folder so we have to create it and must save the `.html` files. we can change this template folder default settings to our own folder in `settings.py`.

#### OR

Django uses templates to separate the **presentation** (HTML) from the **logic** (views and models).

#### **Creating a Template**

Inside your app, create a `templates` directory:

```markdown
myapp/
├── templates/
│   └── myapp/
│       └── home.html
```

For example, `home.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Home Page</title>
</head>
<body>
  <h1>Hello, {{ name }}!</h1>
</body>
</html>
```

#### **Loading the Template in a View**

Instead of returning plain text, we can use `render()`:

```python
from django.shortcuts import render

def home(request):
    context = {'name': 'John'}
    return render(request, 'app_name/home.html', context)
```

*Note*:

* `context` is a dictionary with data passed to the template.
* `{{ name }}` in the template will be replaced with `'John'`.

### **Template Inheritance**

You can create a **base template** to reuse common HTML:

**`base.html`**:

```html
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
  <header>
    <h1>My Site Header</h1>
  </header>
  <main>
    {% block content %}
    {% endblock %}
  </main>
</body>
</html>
```

**`home.html`** extends `base.html`:

```html
{% extends 'base.html' %}

{% block title %}Home{% endblock %}

{% block content %}
  <h2>Welcome, {{ name }}!</h2>
{% endblock %}
```

#### **Rendering Data**

* `{{ variable }}` – output variable’s value.
* `{% for item in list %}` – for loops.
* `{% if condition %}` – conditional statements.

Example:

```html
<ul>
  {% for item in items %}
    <li>{{ item }}</li>
  {% endfor %}
</ul>
```

---

## Models & Databases

### **What are Models?**

Models define the **data structure** of your app – they map to database tables.

A model is a **Python class** that inherits from `django.db.models.Model`.

Example `models.py`:

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    published_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

*Fields*:

* `CharField`, `TextField`, `DateTimeField`, `IntegerField`, etc.
* `auto_now_add=True` sets the date automatically when created.

### **Migrations**

Migrations are Django’s way of updating the database schema.

#### **Create Migrations**

```bash
python manage.py makemigrations
```

#### **Apply Migrations**

```bash
python manage.py migrate
```

#### **Using the Admin Site**

Create a superuser:

```bash
python manage.py createsuperuser
```

Register your models in `admin.py`:

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

#### **Querying the Database**

we can use the **Django ORM** to interact with the database:

```python
# In the Django shell
python manage.py shell

# Example commands:
from myapp.models import Post
Post.objects.all()           # get all posts
Post.objects.filter(title="Django")  # filter by title
Post.objects.get(id=1)       # get a single post
```

---

## Media Handling

### Code to be inserted

**Go `settings.py` and paste following code at the place where static file code is present**

```python
import os

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR,'staticfiles')

STATICFILES_DIRS = [os.path.join(BASE_DIR, 'public/static')]

MEDIA_ROOT = os.path.join(BASE_DIR,'public/static')
MEDIA_URL = '/media/'
```

**Now go to urls.py and paste following code at last**

```python
from django.contrib.staticfiles.urls import staticfiles_urlpatterns
from django.conf import settings
from django.conf.urls.static import static

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,document_root = settings.MEDIA_ROOT)


urlpatterns += staticfiles_urlpatterns()
```

**Now for adding url for media in html using following**

`"/media/{{object_name.variable_name}}"`

---

## Dynamic URL

### For Setting Path

```python
path('delete-recipe/<id>/',delete_recipe, name='Delete Recipe')
```

### For in HTML File

```html
<a href="/delete-recipe/{{r.id}}" class="btn btn-danger"> Delete </a>
```

---

## Search Method

```html
<form method="get" class="container">
  <input type="text" name="search" placeholder="Search..." >
  <button type="submit">Search</button>
  </form>
```

### How to take it in backend

```html
if request.GET.get('search'):
        recipess = Recipe.objects.filter(recipe_name__icontains=request.GET.get('search'))
```

`__icontains` It will check if the particular word is found in given recipe_name string or not

---

## Linking using name of path in html

```html
<a href="{% url 'Register' %}" class="link">Register</a>
```

---

## Storing a password as encrypted

```python
user.set_password(password)
```

### OR

```python
# It automatically encrypt the password
user = User.objects.create_user(
    first_name=first_name,
    last_name=last_name,
    username=username,
    email=email,
    password=password
)
```

---

## Filtering using username

```python
if not User.objects.filter(username=username).exists():
            messages.error(request,"User doesn't exit")
            return redirect('/login/')
```

---

## Authenticating User using built in function

```python
from django.contrib.auth import authenticate, login

user = authenticate(username=username,password=password)

if not user:
    messages.error(request,"Invalid Password")
    return redirect('/login/')
        
login(request,user=user)
return redirect('/recipes/')
```

---

## Setting a view to be used when loged in

```python
from django.contrib.auth.decorators import login_required

@login_required(login_url='/login_url_you_have_created/')

def example(request):
    pass
```

---

## Checking if the User is authenticated or not

```python
if request.user.is_authenticated:
    print(Yes)
else:
    print(No)
```

---

## Sorting all Records

```python
# Ascending Order
Recipe.objects.all().order_by('name_of_variable_you_want_to_sort_with')
# Descending Order
# Just add - sign with the variable name inside the string
Recipe.objects.all().order_by('-name_of_variable_you_want_to_sort_with')
```

---

## Filtering according to certain condition

### 1. Range of Records

```python
# I want to filter top 100 records (Slicing Like String)
Recipe.objects.all().order_by('name_of_variable_you_want_to_sort_with')[0:101]

# Slicing More Examples

# Get first 100
Recipe.objects.all().order_by('name')[:100]

# Get from 50 to 100
Recipe.objects.all().order_by('name')[50:100]

# Get every second object from 0 to 100
Recipe.objects.all().order_by('name')[0:100:2]

# Convert to list for negative slicing
recipes = list(Recipe.objects.all().order_by('name')[:100])
last_10 = recipes[-10:]

```

### 2. Comparison Filtering

#### Equals One

`Recipe.objects.filter(variable_name = 50)`

#### Not Equal one

`Recipe.objects.exclude(variable_name=50)`

#### Greater Than

We use **__gt** with variable name

`Recipe.objects.filter(variable_name__gt=50)`

#### Less Than

We use **__lt** with variable name

`Recipe.objects.filter(variable_name__lt=50)`

#### Greater Than or Equal to

We use **__gte** with variable name

`Recipe.objects.filter(variable_name__gte=50)`

#### Less Than or Equal to

We use **__lte** with variable name

`Recipe.objects.filter(variable_name__lte=50)`

#### In a list of values (like IN in SQL)

We use **__in** with variable name

`Recipe.objects.filter(variable_name__in=[10, 20, 30])`

#### Range (like BETWEEN)

We use **__range** with variable name

`Recipe.objects.filter(variable_name__range=(10, 20))`

#### Is NULL or not

`Recipe.objects.filter(variable_name__isnull=True)   # variable_name IS NULL`

`Recipe.objects.filter(variable_name__isnull=False)  # variable_name IS NOT NULL`

#### Exact match for string (case-sensitive)

`Recipe.objects.filter(variable_name__exact='exact_value')`

#### Case-insensitive exact match

`Recipe.objects.filter(variable_name__iexact='exact_value')`

#### Starts with (Case-sensitive)

`Recipe.objects.filter(variable_name__startswith='abc')`

#### Starts with (Case-insensitive)

`Recipe.objects.filter(variable_name__istartswith='abc')`

#### Ends with (Case-sensitive)

`Recipe.objects.filter(variable_name__endswith='xyz')`

#### Ends with (Case-insensitive)

`Recipe.objects.filter(variable_name__iendswith='xyz')`

#### Contains substring (Case-sensitive)

`Recipe.objects.filter(variable_name__contains='part')`

#### Contains substring (Case-insensitive)

`Recipe.objects.filter(variable_name__icontains='part')`

#### Regular expression match

```python
Recipe.objects.filter(variable_name__regex=r'^abc')
Recipe.objects.filter(variable_name__iregex=r'abc$')  # Case-insensitive regex
```

#### Date/time comparisons (with DateField, DateTimeField)

```python
Recipe.objects.filter(date_field__year=2025)
Recipe.objects.filter(date_field__month=6)
Recipe.objects.filter(date_field__day=9)
```

---

## f
