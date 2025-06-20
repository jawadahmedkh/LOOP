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

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
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

## For seeting Automatic url in html files with Django

```html
<a href="{% url 'url_name_you_have_given_while_setting_up_path'%}" class="btn btn-danger"> Delete </a>
```

## For seeting Automatic url in html files with Django (with parameter)

```html
<a href="{% url 'See Marks' student.student_id %}" class="btn btn-danger"> Delete </a>
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

## Accessing Foreign Key Fields

When you have a foreign key relationship, you can access related fields using the double underscore `__`.

```python
Student.objects.filter(department__department  = 'Civil') 
# Here department is foreign key in Student model and department is field in Department model
```

### Using `__` Fucntion with Foreign Key

```python
Student.objects.filter(department__department__icontains  = 'Civil')
```

---

## Seeing All fields of a record using django (Dict)

```python
Student.objects.filter(student_name__icontains = 'abi').values()
## -----------------------------------------OUTPUT ----------------------------------------------
<QuerySet [{'id': 57, 'department_id': 4, 'student_id_id': 60, 'student_name': 'Tabitha Gutierrez', 'student_age': 44, 'student_email': 'odixon@example.net', 'student_address': '0093 Brittany Port Suite 919\nLake Jacobville, MN 62388'}]>
# Or we can go with specific values
Student.objects.filter(student_name__icontains = 'abi').values('student_name','student_age')
## ---------------------------------------OUTPUT------------------------------------------------
<QuerySet [{'student_name': 'Tabitha Gutierrez', 'student_age': 44}]>
```

---

## Reversing QuerSet

```python
# Reversing the order of a QuerySet
Student.objects.filter(student_name__icontains = 'abi').order_by('student_name').reverse()
# OR
Student.objects.filter(student_name__icontains = 'abi').order_by('student_name')[::-1]
```

---

## Values List

The only difference betwwen `values()` and `values_list()` is that `values()` returns `dict` while `values_list()` returns a list of all values

```python
# Getting a list of values for a specific field
Student.objects.filter(student_name__icontains = 'abi').values_list('student_name', flat=True)
# OR
Student.objects.filter(student_name__icontains = 'abi').values_list('student_name')
# ---------------------------------------OUTPUT------------------------------------------------
<QuerySet ['Tabitha Gutierrez']>
# OR
Student.objects.filter(student_name__icontains = 'abi').values_list('student_name', 'student_age')
# ---------------------------------------OUTPUT------------------------------------------------
<QuerySet [('Tabitha Gutierrez', 44)]>
```

---

## Aggregate in Django

* It works only one column at a time
* Operations on a single column like `SUM`, `AVG`, `COUNT`, `MAX`, `MIN`
* It returns a dictonary

```python
from django.db.models import Count, Sum, Avg, Max, Min

Student.objects.aggregate(Avg('student_age')) # Average Age
Student.objects.aggregate(Min('student_age')) # Minimum Age
Student.objects.aggregate(Max('student_age')) # Maximum Age
Student.objects.aggregate(Sum('student_age')) # Sum of the Ages

```

---

## Annotate in Django

In general we see the count of each one like how much peoples are there of age 20 and etc...

```python
Student.objects.values('student_age').annotate(Count('student_age'))
Student.objects.values('department__department').annotate(Count('department__department'))
Student.objects.values('department').annotate(Count('department'))

# With two counts
Student.objects.values('department', 'student_age').annotate(Count('department'), Count('student_age'))
```

---

## unique_together

It ensures a combination of fields must be unique (i.e., no two model instances can have the same values for all specified fields).

* Prevent duplicate entries (e.g., a book can’t have the same title and author).
* Composite unique constraints (instead of unique=True on a single field).

```python
unique_together = ['student','subject']
```

---

## Admin Site Customization

### Customizing Admin Interface

To customize how models appear in the Django admin interface, you can create a custom admin class.

```python
from django.contrib import admin
from .models import Student
class StudentMarksAdmin(admin.ModelAdmin):
    list_display = ['student','subject','marks']

admin.site.register(Subject_Marks,StudentMarksAdmin) # Registering a new interface of given Table i.e. Student_Marks
```

----

## Pagination

Pagination is used to split large datasets into smaller, manageable chunks.

### Basic Pagination Example

```python
from django.core.paginator import Paginator
def get_student(request):
    queryset = Student.objects.all()
    paginator = Paginator(queryset,20)
    page_number = request.GET.get('page',1)
    page_obj = paginator.get_page(page_number)

    return render(request,'students.html',context={'page':'Get Students', 'student':page_obj})
```

### Template Example

```html
<!-- Paginator  -->
<div class="pagination">
    <span class="step-links">
        {% if student.has_previous %}
            <a href="?page=1">&laquo; first</a>
            <a href="?page={{ student.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ student.number }} of {{ student.paginator.num_pages }}.
        </span>

        {% if student.has_next %}
            <a href="?page={{ student.next_page_number }}">next</a>
            <a href="?page={{ student.paginator.num_pages }}">last &raquo;</a>
        {% endif %}
    </span>
</div>
```

---

## Q in Django

Q objects allow you to build complex queries using logical operators like AND, OR, and NOT.

```python
from django.db.models import Q

f request.GET.get('search'):
        word  = request.GET.get('search')
        queryset = Student.objects.filter( Q(student_name__icontains = word) | Q(department__department__icontains = word))

# This will filter students whose name or department contains the search word
```

---

## Custom User Model

To create a custom user model in Django, you need to define a new model that inherits from `AbstractBaseUser` and `PermissionsMixin`. This allows you to customize the user model while still retaining the built-in authentication features.

### Custom User Model with AbstractBaseUser

When we have to create a custom user model, from scratch, then we use it.

### Setting up django to login with your own chocie of field

Set `USERNAME_FIELD` in your custom user model to the field you want to use for authentication (e.g., email).

```python
USERNAME_FIELD = ['phone_number']
```

### Step 1: Create a Custom User Model

```python
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager, PermissionsMixin
from django.db import models

class CustomUser(AbstractUser):
    username = None
    email = models.EmailField(unique=True)
    phone_number = models.CharField(max_length=100,unique=True)
    user_bio = models.CharField(max_length=50)
    profile_image = models.ImageField(upload_to='profile/')

    USERNAME_FIELD = 'phone_number'
    REQUIRED_FIELDS = []
```

We must have to create a `createsuperuser()` method for our custom user model to create a superuser.

### Step 2: Create a Custom User Manager

```python
from django.contrib.auth.models import BaseUserManager

class UserManager(BaseUserManager):

    def create_user(self, phone_number, password=None, **extra_fields):
        if not phone_number:
            raise ValueError('Phone number is required')

        # Only normalize email if it exists
        email = extra_fields.get('email')
        if email:
            extra_fields['email'] = self.normalize_email(email)

        user = self.model(phone_number=phone_number, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)

        return user

    def create_superuser(self, phone_number, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)
        extra_fields.setdefault('is_active', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self.create_user(phone_number, password, **extra_fields)
```

### Step 3: Tell Django that we have created a custom user model

* Now after creating custom user model and cutomer user model manager we have to tell django that we have created a cutom user model

* So go to `settings.py` add following lines in it:

* `AUTH_USER_MODEL = 'app_name.CustomUser'`

* Now delete all migrations & Database and start from scratch i.e. Run all migrations again.

### Step 4: Run migrations command

`python manage.py makemigratins`
`python manage.py migrate`

After you have created a custom user model, now you have to import User model with `get_user_model()` method.

```python
from django.contrib.auth import get_user_model

User = get_user_model()
```

---

## Slug Fields in Django

* A slug is a short label used to identify a particular resource in a URL-friendly way. It typically consists of letters, numbers, underscores, or hyphens.
* Slugs are often used in URLs to make them more readable and SEO-friendly.
* For example, a blog post titled "Django Tutorial" might have a slug like "django-tutorial", resulting in a URL like `https://example.com/blog/django-tutorial/`.
* Slugs are useful for creating clean, user-friendly URLs that are easy to read and remember.
* In Django, you can create a slug field in your model to automatically generate slugs based on another field (like the title of a blog post) or to allow manual input of slugs.
* Slugs can be used in URL patterns to retrieve specific objects from the database, making it easier to create dynamic and user-friendly URLs.
* Slugs are typically unique within a model, ensuring that each URL corresponds to a specific resource without conflicts.
* Slugs can be generated automatically using Django's `slugify` function, which converts a string into a slug by removing special characters and converting spaces to hyphens.
* Slugs can also be manually set by users, allowing for customization of URLs while still maintaining a consistent format.

### Creating a Slug Field

To create a slug field in your model, you can use the `SlugField` provided by Django.

```python
from django.db import models
from django.utils.text import slugify

class BlogPost(models.Model):
    title = models.CharField(max_length=200)
    slug = models.SlugField(unique=True, blank=True)
    content = models.TextField()

    def save(self, *args, **kwargs):
        if not self.slug:
            self.slug = slugify(self.title)
        super().save(*args, **kwargs)
```

---

## How to work without deleting a data in database

If you want to work with data without deleting it from the database, you can use a **soft delete** approach. This involves adding a field to your model to indicate whether the record is active or deleted, rather than actually removing it from the database.

Soft delete means, we don't actually delete the data from database, actually we write a flag for it `is_deleted`,Now if we have to delete some data, we mark `is_deleted = True` for that data and we are taking data from the database we filter only those who are marked with `is_deleted = False`. That's It.

### Example of Soft Delete

```python
from django.db import models

class MyModel(models.Model):
    name = models.CharField(max_length=100)
    is_deleted = models.BooleanField(default=False) #this is a field we use for soft delete

    def delete(self, *args, **kwargs):
        self.is_deleted = True
        self.save()

```

### Querying Active Records

When querying the database, you can filter out the soft-deleted records:

```python
active_records = MyModel.objects.filter(is_deleted=False)
```

## Problems

Butt here using this method we have to write `is_deleted = False` every time we query the database, so to avoid this we can use a custom manager.

```python
from django.db import models

class RecipeManager(models.Manager):

    def get_queryset(self) -> models.QuerySet:
        return super().get_queryset().filter(is_deleted = False)

```

Now we have to set a line before creating a model:

```python

# Go to model for which we are building a ModelManager and following lines

objects = RecipeManager() # Add your manager name instead of RecipeManager()
admin_objects = models.Manager() # We have also given a custom name to model manager for calling it in queireis like Recipes.admin_objects.all() will return all records including deleted ones
```

---

## ✅ Custom User Model Using `AbstractUser` (Recommended Approach)

### 📁 File: `accounts/models.py`

```python
from django.contrib.auth.models import AbstractUser
from django.db import models
from django.utils import timezone

# ✅ Custom User model inheriting from AbstractUser
class CustomUser(AbstractUser):
    full_name = models.CharField(max_length=255, blank=True)
    date_joined = models.DateTimeField(default=timezone.now)
    is_deleted = models.BooleanField(default=False)  # Optional for soft deletion

    def __str__(self):
        return self.username  # Or self.email if email is your username
```

---

### File: `accounts/manager.py`

```python
from django.contrib.auth.models import BaseUserManager

class UserManager(BaseUserManager):

    def create_user(self, phone_number, password=None, **extra_fields):
        if not phone_number:
            raise ValueError('Phone number is required')

        # Only normalize email if it exists
        email = extra_fields.get('email')
        if email:
            extra_fields['email'] = self.normalize_email(email)

        user = self.model(phone_number=phone_number, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)

        return user

    def create_superuser(self, phone_number, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)
        extra_fields.setdefault('is_active', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self.create_user(phone_number, password, **extra_fields)
```

### 📁 File: `accounts/admin.py`

```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import CustomUser

# ✅ Customizing admin panel for the CustomUser model
class CustomUserAdmin(UserAdmin):
    model = CustomUser
    list_display = ['username', 'email', 'full_name', 'is_staff', 'is_superuser']
    fieldsets = UserAdmin.fieldsets + (
        (None, {'fields': ('full_name', 'is_deleted')}),
    )

admin.site.register(CustomUser, CustomUserAdmin)
```

---

### 📁 File: `settings.py` (add this at top-level)

```python
# ✅ Tell Django to use your custom user model
AUTH_USER_MODEL = 'accounts.CustomUser'
```

---

### 📁 File: Anywhere you use a ForeignKey to user

Instead of:

```python
from django.contrib.auth.models import User
user = models.ForeignKey(User, ...)
```

Use:

```python
from django.conf import settings
user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```

This ensures compatibility with any future user model.

---

### ✅ How to Use in Forms or Views

#### For forms or business logic

```python
from django.contrib.auth import get_user_model

User = get_user_model()
```

---

### 🔁 Run Migrations

Do this **before** you create any other models that refer to the user:

```bash
python manage.py makemigrations accounts
python manage.py migrate
```

---

### 🧑‍💼 Create Superuser

```bash
python manage.py createsuperuser
```

You'll be prompted for username, email, and password.

---

## 🧠 Notes

| Feature             | Value                                                                |
| ------------------- | -------------------------------------------------------------------- |
| Base Class          | `AbstractUser` (inherits all useful fields like `username`, `email`) |
| Field Added         | `full_name`, `is_deleted`                                            |
| Admin Panel         | Fully functional with custom fields                                  |
| Username Field      | Still `username` (easier setup than using email as login)            |
| Use in Models/Views | Always via `settings.AUTH_USER_MODEL` or `get_user_model()`          |

---

## Custom User Using `AbstractBaseUser` and `PermissionsMixin`

> 🧠 Use this if you want to replace Django’s default `username` login with email-based login or add deep custom behavior.

---

### 📁 File: `accounts/models.py`

```python
from django.db import models
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager, PermissionsMixin
from django.utils import timezone

# ✅ STEP 1: Custom User Manager
class CustomUserManager(BaseUserManager):
    def create_user(self, email, password=None, **extra_fields):
        if not email:
            raise ValueError("Email is required")
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)  # Encrypt the password
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        if not extra_fields.get('is_staff'):
            raise ValueError('Superuser must have is_staff=True.')
        if not extra_fields.get('is_superuser'):
            raise ValueError('Superuser must have is_superuser=True.')

        return self.create_user(email, password, **extra_fields)


# ✅ STEP 2: Custom User Model
class CustomUser(AbstractBaseUser, PermissionsMixin):
    email = models.EmailField(unique=True)
    full_name = models.CharField(max_length=255)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)
    date_joined = models.DateTimeField(default=timezone.now)

    USERNAME_FIELD = 'email'         # Use email to log in
    REQUIRED_FIELDS = ['full_name']  # Prompt for this when creating a superuser

    objects = CustomUserManager()    # Attach the custom manager

    def __str__(self):
        return self.email
```

---

### 📁 File: `accounts/admin.py`

```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from django.utils.translation import gettext_lazy as _
from .models import CustomUser

# ✅ Customize admin interface for CustomUser
class CustomUserAdmin(UserAdmin):
    model = CustomUser
    list_display = ('email', 'full_name', 'is_staff', 'is_superuser')
    search_fields = ('email', 'full_name')
    ordering = ('email',)

    fieldsets = (
        (None, {'fields': ('email', 'password')}),
        (_('Personal info'), {'fields': ('full_name',)}),
        (_('Permissions'), {'fields': ('is_active', 'is_staff', 'is_superuser', 'groups', 'user_permissions')}),
        (_('Important dates'), {'fields': ('last_login', 'date_joined')}),
    )

    add_fieldsets = (
        (None, {
            'classes': ('wide',),
            'fields': ('email', 'full_name', 'password1', 'password2', 'is_staff', 'is_active')}
        ),
    )

admin.site.register(CustomUser, CustomUserAdmin)
```

---

### 📁 File: `settings.py`

```python
AUTH_USER_MODEL = 'accounts.CustomUser'
```

---

### 📁 Update All `ForeignKey` Usage to User

Anywhere in models:

```python
from django.conf import settings
user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```

---

### 🧪 Run Migrations

If this is your first time creating the project:

```bash
python manage.py makemigrations accounts
python manage.py migrate
```

If you've already run migrations **before setting** `AUTH_USER_MODEL`, let me know — **you must reset the database** to avoid migration conflicts.

---

### 🧑‍💼 Create a Superuser

```bash
python manage.py createsuperuser
# You will be asked for: email, full name, and password
```

---

### ✅ Use in Views and Forms

```python
from django.contrib.auth import get_user_model
User = get_user_model()

# Example usage
user = User.objects.get(email="someone@example.com")
```

---

## 🧠 Summary Table

| Component           | Description                                           |
| ------------------- | ----------------------------------------------------- |
| `AbstractBaseUser`  | Base user class without username or permissions       |
| `PermissionsMixin`  | Adds `is_superuser`, `groups`, and `user_permissions` |
| `CustomUserManager` | Handles `create_user()` and `create_superuser()`      |
| `AUTH_USER_MODEL`   | Set to `'accounts.CustomUser'` in `settings.py`       |
| Admin Panel         | Fully customized admin registration                   |
| Login With          | `email` (not username)                                |

---

## How to send and Email in Django

To configure **email in Django**, you need to update the settings so Django can use an SMTP server to send emails (e.g. for user registration, password reset, contact forms, etc.).

### 1. Add Email Configuration in `settings.py`

#### Example: Gmail SMTP (Recommended for Development)

```python
# settings.py

# Enable Django to send emails
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

# Your email host credentials
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'youremail@gmail.com'       # Replace with your email
EMAIL_HOST_PASSWORD = 'yourapppassword'       # Not your email password (see below)
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
```

### 2. App Password (if using Gmail)

Google **does not allow** logging in with your Gmail password directly.

Instead:

#### Steps to Get App Password

1. Go to [https://myaccount.google.com/security](https://myaccount.google.com/security)
2. Enable **2-Step Verification**
3. Then go to **App Passwords**
4. Select app: "Mail" → Device: "Other" → Name it like "Django"
5. Copy the 16-character app password and paste it into `EMAIL_HOST_PASSWORD`

### 3. Test Email Sending (Optional)

Create a test view or use Django shell:

```python
from django.core.mail import send_mail

send_mail(
    subject='Test Email',
    message='This is a test email from Django.',
    from_email='youremail@gmail.com',
    recipient_list=['target@example.com'],
    fail_silently=False,
)
```

### 4. Alternative for Development (No SMTP Needed)

Use **console backend** during development to print emails to the terminal:

```python
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

No email is sent, but it shows what would have been sent.

### Other Common SMTP Providers

| Provider | Hostname            | Port | TLS/SSL | Note                      |
| -------- | ------------------- | ---- | ------- | ------------------------- |
| Gmail    | smtp.gmail.com      | 587  | TLS     | Requires app password     |
| Outlook  | smtp.office365.com  | 587  | TLS     | Use Outlook credentials   |
| Yahoo    | smtp.mail.yahoo.com | 587  | TLS     | Also needs app password   |
| Zoho     | smtp.zoho.com       | 587  | TLS     | For business use          |
| Mailtrap | smtp.mailtrap.io    | 2525 | TLS     | Dev email testing service |

---

## Password Reset Functionality in Django

Django has **built-in views** for password reset that handle everything automatically.

### Add These URLs to `urls.py`

```python
from django.contrib.auth import views as auth_views
from django.urls import path

urlpatterns = [
    # Password reset flow
    path('password-reset/', auth_views.PasswordResetView.as_view(), name='password_reset'),
    path('password-reset/done/', auth_views.PasswordResetDoneView.as_view(), name='password_reset_done'),
    path('reset/<uidb64>/<token>/', auth_views.PasswordResetConfirmView.as_view(), name='password_reset_confirm'),
    path('reset/done/', auth_views.PasswordResetCompleteView.as_view(), name='password_reset_complete'),
]
```

### Required Templates (Create these HTML files)

Inside `templates/registration/` directory:

* `password_reset_form.html` – email input form
* `password_reset_done.html` – confirmation email sent
* `password_reset_confirm.html` – new password form
* `password_reset_complete.html` – success page

You can copy and customize Django’s default templates:
[https://github.com/django/django/tree/main/django/contrib/admin/templates/registration](https://github.com/django/django/tree/main/django/contrib/admin/templates/registration)

### Email Settings (in `settings.py`)

Make sure your email settings are configured as shown in previous message.

You can also customize email content:

```python
PASSWORD_RESET_EMAIL_TEMPLATE_NAME = 'emails/password_reset_email.html'
```

Then create that template to style the email.

---

## Email Verification (After Registration) in Django

Django doesn’t do this by default, so we build it manually.

### Generate Email Verification Token

In your `utils.py` or `views.py`:

```python
from django.contrib.auth.tokens import default_token_generator
from django.utils.http import urlsafe_base64_encode
from django.utils.encoding import force_bytes
from django.core.mail import send_mail
from django.urls import reverse

def send_verification_email(request, user):
    uid = urlsafe_base64_encode(force_bytes(user.pk))
    token = default_token_generator.make_token(user)
    link = request.build_absolute_uri(
        reverse('verify_email', kwargs={'uidb64': uid, 'token': token})
    )
    subject = 'Verify your email address'
    message = f'Click the link to verify: {link}'
    send_mail(subject, message, 'noreply@example.com', [user.email])
```

### Add Verify URL

```python
from django.contrib.auth.tokens import default_token_generator
from django.utils.http import urlsafe_base64_decode
from django.contrib.auth import get_user_model
from django.shortcuts import redirect
from django.urls import path

User = get_user_model()

def verify_email(request, uidb64, token):
    try:
        uid = urlsafe_base64_decode(uidb64).decode()
        user = User.objects.get(pk=uid)
    except:
        user = None

    if user and default_token_generator.check_token(user, token):
        user.is_active = True
        user.save()
        return redirect('login')  # or success page
    else:
        return redirect('error_page')  # invalid token

urlpatterns += [
    path('verify-email/<uidb64>/<token>/', verify_email, name='verify_email'),
]
```

### In Registration View

When creating a user, set them as inactive and send verification email:

```python
user = CustomUser.objects.create_user(email=email, password=password, full_name=name, is_active=False)
send_verification_email(request, user)
```

---

## 📩 Final Notes

| Task                 | Use Django Built-in? | Email Needed? |
| -------------------- | -------------------- | ------------- |
| Password Reset       | ✅ Yes                | ✅ Required    |
| Email Verification   | ❌ Manual (above)     | ✅ Required    |
| Welcome/Custom Email | ❌ Manual             | ✅ Optional    |

---

## Django Signals

There are four types of signals in Django:

1. **Pre-Save Signal**: Triggered before a model instance is saved.
2. **Post-Save Signal**: Triggered after a model instance is saved.
3. **Pre-Delete Signal**: Triggered before a model instance is deleted.
4. **Post-Delete Signal**: Triggered after a model instance is deleted.

### Example of Using Signals

```python
from django.db.models.signals import post_save, pre_delete
from django.dispatch import receiver
from django.contrib.auth.models import User

@receiver(post_save, sender=User) # Here we have to give the method you are using as a sender and sender is a Class or model for which are you using these signals.

def user_created(sender, instance, created, **kwargs):
    if created:
        print(f'User {instance.username} has been created.')

@receiver(pre_delete, sender=User)
def user_deleted(sender, instance, **kwargs): # This is a custom method, we made it by ourselves.
    print(f'User {instance.username} is about to be deleted.') # The message which we have to display when the user is deleted.
```

### How to Use Signals

To use signals, you need to import the signal you want to use and the receiver function. The receiver function is decorated with `@receiver` and takes the sender model as an argument.

Signals are mostly used for:

* **Logging**: Track when records are created, updated, or deleted.
* **Notifications**: Send emails or messages when certain actions occur.
* **Data Integrity**: Validate or modify data before saving or deleting.

---

## Sending Email with attachments in Django

To send an email with attachments in Django, you can use the `EmailMessage` class from `django.core.mail`. Here’s how to do it:

```python
from django.core.mail import EmailMessage
from django.conf import settings
from django.core.files import File

def send_email_with_attachment(subject, message, recipient_list, attachment_path):
    # Create the email message
    mail = EmailMessage(
        subject=subject,
        body=message,
        from_email='jawadfarman786@gmail.com',
        to=recipient_list,
    )

    with open('mail_sending.py', 'rb') as f:
                file_data = f.read()
                file_name = 'mail_sending.py'

    mail.attach(file_name, file_data)  # mimetype for Python file

    mail.send()

```

---

## Boosting ORM Qeuries (Writing Raw SQL in Django)

```python
from django.db import connection
def execute_raw_query(query, params=None):
    with connection.cursor() as cursor:
        cursor.execute(query, params or [])
        return cursor.fetchall()

# Example usage
query = "SELECT * FROM myapp_mymodel WHERE age > %s"
params = [30]
results = execute_raw_query(query, params)
# Process results
for row in results:
    print(row)
```

---

## Adding Atomicity on DataBase Queries in Django

```python

from django.db import connection
from django.db import transaction

@transaction.atomic # It is a decorater that make sure all transactions must be run and vice versa.
def execute_raw_query(query, params=None):
    with connection.cursor() as cursor:
        cursor.execute(query, params or [])
        return cursor.fetchall()

# Example usage
query = "SELECT * FROM myapp_mymodel WHERE age > %s"
params = [30]
results = execute_raw_query(query, params)
# Process results
for row in results:
    print(row)

```

---
## Cretinga 
                              **That's IT**
