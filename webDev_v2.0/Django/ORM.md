
https://docs.djangoproject.com/en/4.2/topics/db/queries/#lookups-that-span-relationships

```python
from datetime import date

from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField(default=date.today)
    authors = models.ManyToManyField(Author)
    number_of_comments = models.IntegerField(default=0)
    number_of_pingbacks = models.IntegerField(default=0)
    rating = models.IntegerField(default=5)

    def __str__(self):
        return self.headline
```

This example retrieves all `Entry` objects with a `Blog` whose `name` is `'Beatles Blog'`:

```python
>>> Entry.objects.filter(blog__name="Beatles Blog")
```


This spanning can be as deep as you’d like.

It works backwards, too, by default you refer to a “reverse” relationship in a lookup using the lowercase name of the model.

This example retrieves all `Blog` objects which have at least one `Entry` whose `headline` contains `'Lennon'`:
```python
>>> Blog.objects.filter(entry__headline__contains="Lennon")
```
