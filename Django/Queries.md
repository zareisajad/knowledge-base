### Complex lookups with `Q` objects

```python
from django.db.models import Q
```

Keyword argument queries – in filter() etc. – are “AND”ed together. If you need to execute more complex queries (for example, queries with `OR` statements), you can use Q objects!

`Q` objects can be combined using the `&`, `|`, and `^` operators. When an operator is used on two `Q` objects, it yields a new `Q` object.

```python
Q(question__startswith="Who") | Q(question__startswith="What")
```
