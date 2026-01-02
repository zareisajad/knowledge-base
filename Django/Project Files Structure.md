
## Rule of thumb for apps

Create a new app when the concept has its own lifecycle, rules, and APIs.  
do not add new app when it is tightly coupled to the main app and has no life on its own.

example : `settings` is related to `user` AND means nothing without it therefor belongs to `users` app.

Does this feature make sense without the model? 
if the answer is yes create a new app.

wrong:
This is user-related, so put it in users

right:
Does this feature make sense without the user model?

### Rule of thumb for models

One model per file once the app grows beyond 1–2 models.  
Relationships do NOT determine file boundaries, responsibility does.

```
users/
├── models/
│   ├── profile.py
|	|── profile_link.py
│   ├── settings.py
```

- Can this model gain business rules later?
- Can it be queried independently?
- Can it have its own API?

If yes separate file.

### Rule of thumb for serializers

```
users/
├── serializers/
│   ├── __init__.py
│   ├── profile.py
│   ├── settings.py
```

Group serializers by domain concept, not strictly by model.
But if a model has a few small related serializers, it’s okay to keep them in the same file.


### Rule of thumb for views

Group views by feature/domain, not by HTTP method or model.
Keep related endpoints in the same file if the file stays manageable.  
Split files only when complexity grows or there are multiple independent APIs and endpoints are unrelated (different models, business logic, permissions).

```
├── views/
│   ├── profile.py
│   ├── settings.py
```

### Recommended Django Project Tree

```
cocode/
├── manage.py
├── config/   
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── authentication/           # Login/Register/JWT
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py 
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│
├── users/               
│   ├── __init__.py
│   ├── apps.py          
|	│   ├── models/           # examples 
│   │   ├── __init__.py  
│   │   ├── profile.py
│   │   └── settings.py
│   ├── serializers/
│   │   ├── __init__.py       
│   │   ├── profile.py      
│   │   ├── settings.py       # UserSettingsSerializer
│   ├── views/
│   │   ├── __init__.py      
│   │   ├── profile.py        
│   │   ├── settings.py
│   │   └── public_user.py
│   ├── signals.py
│   ├── urls.py
│   └── tests/
│       ├── __init__.py
│       ├── test_views.py
│       ├── test_models.py
│       └── test_serializers.py
│
```