# Django16 and here are steps for creating django project

 ### 1. First we install django in python

**Command:**

>pip install django

### 2. We create our django project

**Command:**

>django-admin startproject core .

For name you can add the name of the project `.` tells django-admin to install django app into the same folder
for this course we name our project core

###  3. Run migrations

**Command:**

>python manage.py migrate

Migration are for install databse default table in sqlite and its file will be created

###  4. We can create a super user from which we can access the admin panel

**Command:**

>python manage.py createsuperuser

### 5. Now we can start our django project:

**Command:**

>python manage.py runserver

This will start our server. and we can see our app in the browser on `localhost:8000`

### 6. From here we can login to our python admin panel (`localhost:8000/admin`) from check everything.

Username and password that we have created at the time of creating supersuer

### 7. In code we can add our django app

**Command:**

>python manage.py startapp pages

For this course we have created a app name of pages in which we can store our static pages
like **home, about and contact**

### 8. After installing the appliaction we have to add our app in django settings in our main project file `(core -> setting)`.

We add our app name at the bottom of `INSTALLED_APPS` list.

**Code:**

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'pages',
]
```

### 9. Now we can create templete for pages

#### 9.1. For template pages first we have to crate a template folder at the start:

**Project:**

```
-core
-pages
-template 
```

#### 9.2. After that we have to add template to setting file `(core -> setting)`:

**Code:**

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['templates'], # here we add our template folder
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
```


#### 9.3. In template file the we can add the `base.html`
the base.html contain all the shared code 

**Example: **
```
navbar
header
footer
```

#### 9.4.  Then we will create a pages folder in the templates:

**Project:**
```
-core
-pages
-template
--base.html
--pages
```

In this pages files we add our `home.html, about.html, contact.html`

**Project:**
```
-core
-pages
-template
--base.html
--pages
---home.html
---about.html
---contact.html
```

### 10. after that we can go into our views in pages file and add the view of the home, about and contact `(pages -> view)`

**Code:**
```
from django.shortcuts import render

def home(request):
    return render(request, "pages/home.html")

def about(request):
    return render(request, "pages/about.html")

def contact(request):
    return render(request, "pages/contact.html")`
```


### 11. Now we can create url of the pages `(core -> urls)`

**Code:**
```
from django.contrib import admin
from django.urls import path
from pages.views import home, about, contact

urlpatterns = [
    path('', home, name='home'),
    path('home', home, name='home'),
    path('about', about, name='about'),
    path('contact', contact, name='contact'),
    path('admin/', admin.site.urls),
]
```

### 12. now we can see the home, about, contact url can work on `localhost:8000`

**Urls:**
```
localhost:8000
localhost:8000/home
localhost:8000/about
localhost:8000/contact
```

### 13. Now we have add static file for loading or css, js and image files

#### 13.1. First we add static folder in or main folder

**Project:**
```
-core
-pages
-template 
-static
```

#### 13.2. We add static folder link in our settings `(core -> setting)`
```
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```

#### 13.3 We can add static into our template:

**Example:**

`<link rel="stylesheet" href="{% static 'css/style.css' %}">`

Or css is in our css folder in static

and likewise image will be:

`<img src="{% static 'home.jpeg' %}" width="500px" height="350px">`


### 14. Now we will add blog for load data from our database


### 15. First we create or blog app:

**Command:**

>python manage.py startapp pages


### 16. We will add our app in our setting folder `(core -> setting)`
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'pages',
    'blog',
]
```


### 17. Add model in blog `(blog -> model)`

**Code:**
```
from django.db import models

class Blog(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    image = models.CharField(max_length=512, null=True)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title`
```

### 18: Then run makemigrations for check model change

**Command:**

>python manage.py makemigrations


### 19. After making migration add these migration to database

**Command:**

>python manage.py migrate


### 20. Add blog into the admin for accessing in the admin panel `(blog -> admin)`

**Code:**
```
from django.contrib import admin
from .models import Blog

class BlogAdmin(admin.ModelAdmin):
    list_display = ('title', 'created_at')
    search_fields = ('title', 'content')
    ordering = ('created_at', )

admin.site.register(Blog, BlogAdmin)`
```

### 21. Now we can read, create, update and delete in django admin (`localhost:8000/admin`)


### 22. After that we will crate our blog template (in template folder)
```
-core
-pages
-template
--base.html
--pages
---home.html
---about.html
---contact.html
---blog.html
---post.html
-static
```

### 23 -> now we will create it views `(blog -> view)`
**Code:**
```
def blog(request):
    return render(request, "pages/blog.html")

def post(request, id):
    return render(request, "pages/post.html")
```

### 24. now add url of blog `(core -> urls)`

**Code:**
```
from django.contrib import admin
from django.urls import path
from pages.views import home, about, contact
from blog.views import blog, post

urlpatterns = [
    path('', home, name='home'),
    path('home', home, name='home'),
    path('about', about, name='about'),
    path('contact', contact, name='contact'),
    path('blog', blog, name='blog'),
    path('post/<int:id>/', post, name='post'),
    path('admin/', admin.site.urls),
]
```

In post we passed id in url and we can get it through a argument in view

### 25. Now we have to fetch data from db and pass it to our template `(blog -> url)`

**Code:**
```
from django.shortcuts import render, get_object_or_404
from .models import Blog

def blog(request):
    blogs = Blog.objects.all()
    context = {
        'blogs': blogs
    }
    return render(request, "pages/blog.html", context)

def post(request, id):
    blog = get_object_or_404(Blog, id=id)
    context = {
        'blog': blog
    }
    return render(request, "pages/post.html", context)`
```


### 26. now we can render the data into our template:

**Example: **

`(template -> post)`

```
{% extends 'base.html' %}

{% block content %}
    <div class="heading">
      <h1>{{ blog.title }}</h1>
    </div>
    <div class="album py-5 bg-light">
        <div class="container">
            <div class="post-image">
              <img src="{{ blog.image }}" alt="img" width="1200px" height="800px">
            </div>
            <div class="content">
              {% autoescape off %}
                  {{ blog.content|safe }}
              {% endautoescape %}
            </div>
        </div>
      </div>
{% endblock %}
```
and we can create a loop for getting all blog in template

**Example: **

`(template blog)`

```
{% extends 'base.html' %}

{% block content %}
    <div class="heading">
      <h1>Blog</h1>
    </div>
    <div class="album py-5 bg-light">
        <div class="container">
          <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
            {% for blog in blogs %}
                  <img src="{{ blog.image }}" alt="" width="100%" height="225">
                  <div class="card-body">
                    <h3>{{ blog.title }}</h3>
                    <p class="card-text">
                      {% autoescape off %}
                        {{ blog.content|slice:":150" }}
                      {% endautoescape %}
                    </p>
            {% endfor %}
          </div>
        </div>
      </div>
{% endblock %}
```
