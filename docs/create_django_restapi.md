# Create a REST API with Django
## Setting up Django
Install Django
```shell
pip install django
```
Start a new project
```shell
django-admin startproject lomi_app
```
Create an app for the API
```shell
cd lomi_app
python manage.py startapp lomi_api
```
Register the lomi_api app with the mysite project in `lomi_app/settings.py`
```python
INSTALLED_APPS = [
    'lomi_api.apps.LomiApiConfig',
    ... # Leave all the other INSTALLED_APPS
]
```
Add existing database to `lomi_app/settings.py` (otherwise Django will create a new SQLite db)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',

        'NAME': sec.db_name,

        'USER': sec.db_user,

        'PASSWORD': sec.db_password,

        'HOST': sec.db_host,

        'PORT': sec.db_port,
    }
}
```
Migrate Django Models
```shell
python manage.py migrate
```
```bash
Operations to perform:                                                                                                                                                                                               Apply all migrations: admin, auth, contenttypes, sessions                                                                                                                                                        Running migrations:                                                                                                                                                                                                  Applying contenttypes.0001_initial... OK                                                                                                                                                                           Applying auth.0001_initial... OK                                                                                                                                                                                   Applying admin.0001_initial... OK                                                                                                                                                                                  Applying admin.0002_logentry_remove_auto_add... OK                                                                                                                                                                 Applying admin.0003_logentry_add_action_flag_choices... OK                                                                                                                                                         Applying contenttypes.0002_remove_content_type_name... OK                                                                                                                                                          Applying auth.0002_alter_permission_name_max_length... OK                                                                                                                                                          Applying auth.0003_alter_user_email_max_length... OK                                                                                                                                                               Applying auth.0004_alter_user_username_opts... OK                                                                                                                                                                  Applying auth.0005_alter_user_last_login_null... OK                                                                                                                                                                Applying auth.0006_require_contenttypes_0002... OK                                                                                                                                                                 Applying auth.0007_alter_validators_add_error_messages... OK                                                                                                                                                       Applying auth.0008_alter_user_username_max_length... OK                                                                                                                                                            Applying auth.0009_alter_user_last_name_max_length... OK                                                                                                                                                           Applying auth.0010_alter_group_name_max_length... OK                                                                                                                                                               Applying auth.0011_update_proxy_permissions... OK                                                                                                                                                                  Applying auth.0012_alter_user_first_name_max_length... OK                                                                                                                                                          Applying sessions.0001_initial... OK 
```
In order to use the Django Admin Interface we need to create a super user
```shell
python manage.py createsuperuser
```
```bash
Username (leave blank to use 'falk'):                                                                                                                                                                              Email address: mail@falkzeh.io                                                                                                                                                                                     Password:                                                                                                                                                                                                          Password (again):                                                                                                                                                                                                  Superuser created successfully.
```
Create a model in `lomi_api/models.py`. First we're creating a databse of dogs.
```python
from django.db import models

class dogs(models.Model):
    dog_name = models.CharField(max_length=60)
    
    def __str__(self):
        return self.dog_name
```
Migrate changes
```shell
python manage.py makemigrations
```
```bash
Migrations for 'lomi_api':                                                                                                                                                                                           lomi_api/migrations/0001_initial.py                                                                                                                                                                                  - Create model dogs 
```
```shell
python manage.py migrate
```
```bash
Operations to perform:                                                                                                                                                                                               Apply all migrations: admin, auth, contenttypes, lomi_api, sessions                                                                                                                                              Running migrations:                                                                                                                                                                                                  Applying lomi_api.0001_initial... OK 
```
To make the changes visible in the admin panel add these lines to `lomi_api/admin.py`
```python
from django.contrib import admin
from .models import dogs

admin.site.register(dogs)
```
Run the server
```shell
python manage.py runserver
```
You are now able to add Dogs via Admin Panel. To set up the REST framework we need to install it
```shell
pip install djangorestframework
```
Let Django know we installed the REST framework in `lomi_app/settings.py`
```python
from djangorestframework import rest_framework
...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'lomi_api.apps.LomiApiConfig',
    'rest_framework',
]
```
