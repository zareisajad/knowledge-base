### Complex lookups with `Q` objects

```python
from django.db.models import Q
```

Keyword argument queries – in filter() etc. – are “AND”ed together. If you need to execute more complex queries (for example, queries with `OR` statements), you can use Q objects!

`Q` objects can be combined using the `&`, `|`, and `^` operators. When an operator is used on two `Q` objects, it yields a new `Q` object.

```python
Q(question__startswith="Who") | Q(question__startswith="What")
```

Each lookup function that takes keyword-arguments (filter(), get(), exclude() ) can also be passed one or more `Q` objects as positional (not-named) arguments. If you provide multiple `Q` object arguments to a lookup function, the arguments will be “AND”ed together. For example: