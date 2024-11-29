# MyBloggingApplicationUsingDjango


## Overview
This is a simple blogging application built using Django. It allows users to view a list of blog posts and read individual posts. 

### Features
1. Display a list of blog posts with truncated content.
2. View detailed content of a single blog post.

---

## Project Structure

### Project: `blog`
The main Django project containing the global settings, URLs, and configurations.

### Application: `posts`
An application within the project responsible for managing blog posts.

---

## Implementation

### 1. `urls.py` in Project

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('posts.urls')),
]
```

This file maps the base URL to the `posts` application.

### 2. `urls.py` in Application

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('post/<str:pk>', views.post, name='post'),
]
```

Here, we define:
- The root path (`''`) for the blog post list.
- A dynamic path (`post/<str:pk>`) for viewing a specific post by ID.

### 3. `views.py`

```python
from django.shortcuts import render
from .models import Post

def index(request):
    posts = Post.objects.all() 
    return render(request, 'index.html', {'posts': posts})

def post(request, pk):
    posts = Post.objects.get(id=pk)
    return render(request, 'posts.html', {'posts': posts})
```

**Functions:**
- `index`: Fetches all posts from the database and renders them in the `index.html` template.
- `post`: Fetches a specific post by its ID and renders it in the `posts.html` template.

### 4. `models.py`

```python
from django.db import models
from datetime import datetime

class Post(models.Model):
    title = models.CharField(max_length=200)
    body = models.CharField(max_length=1000000)
    created_at = models.DateTimeField(default=datetime.now, blank=True)
```

**Post Model:**
- `title`: Title of the blog post.
- `body`: Content of the blog post.
- `created_at`: Timestamp when the post was created.

### 5. Templates

#### `index.html`

```html
<div class="header">
  <h2>ERIT's Blog</h2>
</div>
<div class="row">
  <div class="leftcolumn">
    {% for post in posts reversed %}
    <div class="card">
      <a style="text-decoration: none; color: black;" href="/post/{{post.id}}">
        <h2>{{post.title}}</h2>
        <h5>{{post.created_at}}</h5>
        <p>{{post.body|truncatewords:20}}</p>
      </a>
    </div>
    {% endfor %}
  </div>
</div>
```

Displays a list of all blog posts with truncated content.

#### `posts.html`

```html
<div class="header">
  <h2>ERIT's Blog</h2>
</div>
<div class="row">
  <div class="leftcolumn">
    <div class="card">
        <h1>{{posts.title}}</h1>
        <h5>{{posts.created_at}}</h5>
        <p>{{posts.body}}</p>
    </div>
  </div>
</div>
```

Displays the detailed content of a specific blog post.

---

## Setup Instructions

1. **Create Django Project:**
   ```bash
   django-admin startproject blog
   cd blog
   ```

2. **Create Django Application:**
   ```bash
   python manage.py startapp posts
   ```

3. **Register `posts` in `settings.py`:**
   ```python
   INSTALLED_APPS = [
       ...
       'posts',
   ]
   ```

4. **Apply Migrations:**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

5. **Run the Server:**
   ```bash
   python manage.py runserver
   ```

---

## Author
This project was built as an example of a basic blogging platform using Django.
