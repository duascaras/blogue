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

#### Limiting Results

- You can limit results by using Python array-slicing. For example:
```
Post.objects.all()[:5]
```

#### Q Objects 

- Field lookups using filter() are joined with a SQL _AND_ operator. 

- For example, filter(field1='foo ', field2='bar') will retrieve objects where field1 is _foo_ and field2 is _bar_. If you need to build more complex queries, such as queries with _OR_ statements, you can use Q objects.

- A Q object allows you to encapsulate a collection of field lookups. You can compose statements by combining Q objects with the & (and), | (or), and ^ (xor) operators.
```
from django.db.models import Q
starts_who = Q(title__istartswith='who')
starts_why = Q(title__istartswith='why')
Post.objects.filter(starts_who | starts_why) # Bulding a OR statement
```

#### QuerySets evalutation

- QuerySet doestn't involve database activity until it is evaluated; 

- QuerySets will usually return another unevaluated QuerySet;

- When a QuerySet is evaluated, it translates into a SQL query to the database;

- QuerySets are only evaluated in the following cases:
```
• The first time you iterate over them
• When you slice them, for instance, Post.objects.all()[:3]
• When you pickle or cache them
• When you call repr() or len() on them
• When you explicitly call list() on them
• When you test them in a statement, such as bool(), or, and, or if
```

## General Notes

- When starting the development on a new pc, I needed to run the migrations command again.

```
python manage.py migrations
```

> [!NOTE]
> Write about it. Why we need to ran the migrations again.


