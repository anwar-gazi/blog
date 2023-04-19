---
layout: post
title:  "Customizing Django Auth: Custom User Model"
date:   2021-07-31 10:00:00 +0600
img: post/django-custom-user-model.png
categories: django tutorials
comments: true
---
## Common Problems 

#### django.core.exceptions.ImproperlyConfigured: AUTH_USER_MODEL refers to model 'myapp.MyUserModel' that has not been installed

After writing your custom user model and specifying it in django settings, you run `makemigrations` and whoa! It fails!

 - Probable reason is you didn't specify your custom user model in admin.py
 - You may have specified `abstract = True` in your custom user model Meta class. Remove it, so that it should look like:
```python
class Meta:
    verbose_name = _('user')
    verbose_name_plural = _('users')
    # abstract = True remove this line if exists
```
[Check the django auth doc: custom user model, a full example](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#a-full-example)

