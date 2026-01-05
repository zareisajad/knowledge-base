## Complex lookups with `Q` objects[¶](https://docs.djangoproject.com/en/6.0/topics/db/queries/#complex-lookups-with-q-objects "Link to this heading")

Keyword argument queries – in filter() etc. – are “AND”ed together. If you need to execute more complex queries (for example, queries with `OR` statements), you can use Q objects!

A Q object is an object used to encapsulate a collection of keyword arguments. These keyword arguments are specified as in “Field lookups” above.

For example, this `Q` object encapsulates a single `LIKE` query:

from django.db.models import Q

Q(question__startswith="What")

`Q` objects can be combined using the `&`, `|`, and `^` operators. When an operator is used on two `Q` objects, it yields a new `Q` object.

For example, this statement yields a single `Q` object that represents the “OR” of two `"question__startswith"` queries:

Q(question__startswith="Who") | Q(question__startswith="What")

This is equivalent to the following SQL `WHERE` clause:

WHERE question LIKE 'Who%' OR question LIKE 'What%'

You can compose statements of arbitrary complexity by combining `Q` objects with the `&`, `|`, and `^` operators and use parenthetical grouping. Also, `Q` objects can be negated using the `~` operator, allowing for combined lookups that combine both a normal query and a negated (`NOT`) query:

Q(question__startswith="Who") | ~Q(pub_date__year=2005)

Each lookup function that takes keyword-arguments (e.g. [`filter()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.filter "django.db.models.query.QuerySet.filter"), [`exclude()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.exclude "django.db.models.query.QuerySet.exclude"), [`get()`](https://docs.djangoproject.com/en/6.0/ref/models/querysets/#django.db.models.query.QuerySet.get "django.db.models.query.QuerySet.get")) can also be passed one or more `Q` objects as positional (not-named) arguments. If you provide multiple `Q` object arguments to a lookup function, the arguments will be “AND”ed together. For example:

Poll.objects.get(
    Q(question__startswith="Who"),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
)

… roughly translates into the SQL:

SELECT * from polls WHERE question LIKE 'Who%'
    AND (pub_date = '2005-05-02' OR pub_date = '2005-05-06')

Lookup functions can mix the use of `Q` objects and keyword arguments. All arguments provided to a lookup function (be they keyword arguments or `Q` objects) are “AND”ed together. However, if a `Q` object is provided, it must precede the definition of any keyword arguments. For example:

Poll.objects.get(
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
    question__startswith="Who",
)

… would be a valid query, equivalent to the previous example; but:

# INVALID QUERY
Poll.objects.get(
    question__startswith="Who",
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
)