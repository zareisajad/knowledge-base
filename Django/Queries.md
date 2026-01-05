
#### Field lookups

Field lookups are how you specify the meat of an SQL `WHERE` clause. They’re specified as keyword arguments to the [`QuerySet`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet "django.db.models.query.QuerySet") methods [`filter()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.filter "django.db.models.query.QuerySet.filter"), [`exclude()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.exclude "django.db.models.query.QuerySet.exclude") and [`get()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.get "django.db.models.query.QuerySet.get").

Basic lookups keyword arguments take the form `field__lookuptype=value`.

```python
>>> Entry.objects.filter(pub_date__lte="2006-01-01")
>>> Entry.objects.get(headline__exact="Cat bites dog")
>>> #case-insensitive match
>>> Blog.objects.get(name__iexact="beatles blog")
>>> # Case-sensitive
>>> Entry.objects.get(headline__contains="Lennon")
```

[`startswith`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#std-fieldlookup-startswith), [`endswith`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#std-fieldlookup-endswith)

Starts-with and ends-with search, respectively. There are also case-insensitive versions called [`istartswith`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#std-fieldlookup-istartswith) and [`iendswith`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#std-fieldlookup-iendswith).

### Complex lookups with `Q` objects

```python
from django.db.models import Q
```

Keyword argument queries – in filter() etc. – are “AND”ed together. If you need to execute more complex queries (for example, queries with `OR` statements), you can use Q objects!

`Q` objects can be combined using the `&`, `|`, and `^` operators. When an operator is used on two `Q` objects, it yields a new `Q` object.

```python
Q(question__startswith="Who") | Q(question__startswith="What")
```

Each lookup function that takes keyword-arguments ( filter(), get(), exclude() ) can also be passed one or more `Q` objects as positional (not-named) arguments. If you provide multiple `Q` object arguments to a lookup function, the arguments will be “AND”ed together. For example:

```python
Poll.objects.get(
    Q(question__startswith="Who"),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
)
```