# Blogue

- Blogue = The Project
- Gueblo = The App

> [!NOTE]
> TODO: Write about the Blog

## Django Overview

- Follows the MTV (Model-Template-View) pattern.

- Similar to MVC, where the template acts as the view and the framework itself as the controller.

- Model: Logical data structure and is the data handler between the database and the view.

- View: Communicates with the database via the model and transfer data to the template for viewing. 

- Template: Presentation layer, using a plain-text system that keeps everything that the browser renders.

## Management commands

```
// To create the file structure for a new Django project named mysite (for example):
django-admin startproject mysite 

// To create the file structure for a new Django application named blog (for example):
python manage.py startapp blog

// To apply all database migrations:
python manage.py migrate

// To create migrations for the models of the blog application:
python manage.py makemigrations blog

// To view the SQL statements that will be executed with the first migration of the blog application:
python manage.py sqlmigrate blog 0001

// To run the Django development server:
python manage.py runserver

// To run the development server specifying host/port and settings file:
python manage.py runserver 127.0.0.1:8001 --settings=mysite.settings

//To run the Django shell:
python manage.py shell

To create a superuser using the Django authentication framework:
python manage.py createsuperuser
```

## QuerySets and managers

- The Django ORM is a way of interact with the database in a Pythonic way, instead of writing raw SQL queries.

- Django can work with multiple databases at a time. We can define it inside our settings.py file.

- The Django ORM is based on QuerySets. 
    - A QuerySet is a collection of database queries; 
    - We can define filters to narrow down the query results based on given parameters;
    - The QuerySet equates to a SELECT SQL statement; and filters are limiting SQL clauses such as WHERE or LIMIT;

### Most Used QuerySets

```
all() # Returns all objects from the database.

filter(**kwargs) # Filters objects based on conditions.

exclude(**kwargs) # Returns objects that do not match the given conditions.

get(**kwargs) # Retrieves a single object; raises an error if multiple or none are found.

get_or_create()

order_by(*fields) # Orders results by specified fields.

distinct() # Removes duplicates from the QuerySet.

count() # Returns the number of objects in the QuerySet.

first() / last() # Retrieves the first or last object from the QuerySet.

exists() # Checks if any record matches the QuerySet.

values(*fields) # Returns dictionaries instead of model instances.

values_list(*fields, flat=False) # Returns tuples or a flat list of values.

update(**kwargs) # Updates multiple objects at once.

delete() # Deletes objects matching the QuerySet.

bulk_create([obj1, obj2, ...]) # Inserts multiple objects in a single query.

order_by("?") # Returns objects in a random order.

select_related(*fields) # Optimizes queries for ForeignKey relationships.

prefetch_related(*fields) # Optimizes queries for ManyToMany relationships.
```

### Limiting Results

- We can limit results by using Python array-slicing. For example:
```
Post.objects.all()[:5]
```

### Q Objects 

- Field lookups using filter() are joined with a SQL _AND_ operator. 

- For example, filter(field1='foo ', field2='bar') will retrieve objects where field1 is _foo_ and field2 is _bar_. If you need to build more complex queries, such as queries with _OR_ statements, you can use Q objects.

- A Q object allows you to encapsulate a collection of field lookups. You can compose statements by combining Q objects with the & (and), | (or), and ^ (xor) operators.
``` python shell
from django.db.models import Q
starts_who = Q(title__istartswith='who')
starts_why = Q(title__istartswith='why')
Post.objects.filter(starts_who | starts_why) # Bulding a OR statement
```

### QuerySets evalutation

- QuerySet doestn't involve database activity until it is evaluated; 

- QuerySets will usually return another unevaluated QuerySet;

- When a QuerySet is evaluated, it translates into a SQL query to the database;

- QuerySets are only evaluated in the following cases:
    - The first time you iterate over them
    - When you slice them, for instance, Post.objects.all()[:3]
    - When you pickle or cache them
    - When you call repr() or len() on them
    - When you explicitly call list() on them
    - When you test them in a statement, such as bool(), or, and, or if

## Django Views and URLs

- A Django view is just a Python function that receives a web request and returns a web responde.

- The responde normally is HTML code.

- We can create a URL file to add our urlpatterns (at the app level).

- We need to add the url patterns created to our urls file (at the project level).
```
# For example:
urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls', namespace='blog')), # This new line
]
```

- Namespaces have to be unique across the entire project.
    - We can refer to our app URLs easily by using the namespace followed by a colon and the URL name.
    - For example, blog:post_list and blog:post_detail.

- Templates define how the data is displayed.
    - Usually written in HTML in combination with Django template language.
    - The template language is based on _template tags_, _template variables_ and _template filters_.
        - Template tags control the rendering of the template and look like this: {% tag %}.
        - Template variables get replaced with values when the template is rendered and look like this: {{ variable }}.
        - Template filters allow you to modify variables for display and look like this: {{ variable|filter }}.

- We generally have a base.html file
    - Other html files can|should inherit from the base template.

- We should use the {% url %} template tag provided by Django.
    - This template allows build URLs dynamically by their name.
    - Follows the DRY principle by not having to hard-code URLs in the templates.

```
# For example:
<a href="{% url 'gueblo:post_detail' post.id %}">
```

### The Request/Response cycle

![Alt text](/home/gustavo/Repos/blogue/docs/request_response_cycle.png "Request/Response Cycle")

> [!NOTE]
> This schema doesn't include Django middleware for the sake of simplicity.

## General Notes

- When starting the development on a new pc, I needed to run the migrations command again.

```
python manage.py migrations
```

> [!NOTE]
> Write about it. Why we need to ran the migrations again.


