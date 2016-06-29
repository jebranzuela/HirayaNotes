Welcome
=======


This site will cover the following:

* Vagrant
* Django

## Vagrant

###Installation

1. Download and run [Virtualbox installer](https://www.virtualbox.org/wiki/Downloads "Virtualbox installer")

2. Download and run [Vagrant installer](https://www.vagrantup.com/downloads.html "Vagrant installer")


3. Open BIOS and check if Intel Virtualization Technology is enabled.
> For other processors, search how they handle virtualization. It might be different compared to Intel.

4. Continue the tutorial with this [site](http://vajrasky.net/2015/01/setup-django-with-python-3-and-nginx-in-vagrant/ "Setup Django with Vagrant")

### Important Commands (Vagrant)

1. `vagrant up` - runs virtual machine
2. `vagrant ssh` - use to access virtual machine
3. `logout` - exits virtual machine (note that it does not stops the virtual machine)
4. `vagrant halt` - stops virtual machine

## Django and Dependencies

### Installation

Follow the tutorial from the previous link [here](http://vajrasky.net/2015/01/setup-django-with-python-3-and-nginx-in-vagrant/ "Setup Django with Vagrant")

### Important Commands (Django)

#### Nginx 

1. `sudo vim /etc/nginx/sites-available/default` - edit settings for Nginx
  * This will be used mainly to edit `location /media` and `location/static`. You can change the file path for `media` and `static` files as you wish. Just remember to put all the assets of your project here.

2. `sudo restart nginx restart` - restarts Nginx to reflect the changes you made.

3. `nginx -t` - use to check errors for Nginx.

#### Virtual Environment

1. `source opt/project_env/bin/activate` - opens Python virtual environment.
> Note that `project_env` is from the installation tutorial earlier. You can change it to whatever you want as long as it is the same on what you used in the installation.

2. `vim /vagrant/project_project/project_project/settings.py` - always change the `DATABASES` part to use PostgreSQL with
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'project_db',
        'USER': 'project_user',
        'PASSWORD': 'yourpassword',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

#### Django

##### Tutorial 01

1. `django-admin startproject 'project_name'` - create a new project

2. `python manage.py runserver` - runs the webserver
>To access the web server, type 192.168.33.10 (from the installation) on your browser

3. `python manage.py startapp polls` - create a new app

##### Tutorial 02

>`test_project` refers to the folder created after running `django-admin startproject test_project`
>`test_app` refers to the folder created after running `python manage.py startapp test_app`

1. `python manage.py migrate` - used to create migration files and apply changes

2. `sudo vim test_project/settings.py` - edit `INSTALLED_APPS` to include an app by adding `name_of_app.apps.name_of_appConfig`

3. `python manage.py makemigrations polls` - create a migration file after you made changes to the app. Run `python manage.py' migrate after

4. `python manage.py shell` - open the Python shell where you can import your Django files.

5. `object.save()` - saves the object in the database

6. `model_name.objects.all()` - display all model objects in the database

7. `model_name.objects.filter(args)` - display all model objects in the database that satisfies the argument/s

8. `python manage.py createsuperuser` - create a user that can access the django admin panel

9. `sudo vim test_app/admin.py` - edit to add a model in the admin panel by adding `admin.site.register(model_name)`

10. `python manage.py collectstatic` - get static files from Django. It is mainly used to get the css file for the admin panel. Make sure to put the file on the static folder you set for Nginx

##### Tutorial 03

1. `from .models import model_name` - put this on views.py to use models inside it.

2. `mkdir test_app/templates/test_app` - directory where you put the html files for views

3. `return render(request, 'test_app/name.html', passed_variables)` - used to call the html file when requested where passed_variables is a list of tuples formatted as
>`name_in_html: variable_in_views`

4. `get_object_or_404(Object, pk=object_id)` - check if object exists on the database 

5. `app_name = 'name_of_app'` - used for better url routing

6. `<li><a href="{% url 'app_name:url_name' params %}">{{ text }}</a></li>` - link `text` to the url

##### Using forms

1. `forms.py` - define Form classes here to use in `views.py`

2. `{{ form }}` - creates a form for all the fields you defined on `forms.py`

3. `{{ form.field_name }}` - creates a form for the specified field only

##### Logging

1. `formatters` - used to style the output written on the console/file

2. `handlers` - dictates what will happen when a log output occurs

3. `loggers` - assigns the parts of your projects to specific handler/s

##### Celery

1. Go to this [site](https://realpython.com/blog/python/asynchronous-tasks-with-django-and-celery/ "Celery Tutorial") to setup Celery and Redis
> Use `sudo aptitude install redis-server` to install redis

2. Do not forget to add the task in `settings.py`
```python
CELERYBEAT_SCHEDULE = {
    'task_name': {
        'task': 'task_name',
        'schedule': timedelta(your_preferred_time),
        'args': () #if the function needs arguments
    },
}
```

#### REST Framework

##### Serialization

1. `pip install djangorestframework` - to install REST framework

2. `serializer = ModelNameSerializer(model_instance)` - translate model instance to native datatypes
> Use `serializer.data` to view the fields of the model

3. `content = JSONRenderer().render(serializer.data)` - convert the data into `json` format

4. To deserialize the data:
```python
stream = BytesIO(content)
data = JSONParser().parse(stream)
serializer = ModelNameSerializer(data=data)
serializer.is_valid()
serializer.validated_data
serializer.save()
```

###### Modules

1. `from snippets.serializers import SnippetSerializer`

2. `from rest_framework.renderers import JSONRenderer`

3. `from rest_framework.parsers import JSONParser`

4. `from django.utils.six import BytesIO`

##### Requests and Responses

1. `serializer.is_valid()` - checks if the data are valid

2. `@api_view(['GET', 'POST', 'PUT', 'DELETE'])` - dictates what requests a view function will handle

3. `return Response(serializer.data, status = status_code)` -  renders to content type as requested by the client
> `serializer` will be the output after running ModelNameSerializer()
> `serializer.error` will give a list of errors when serializer is not valid
> `status` is an optional argument

4. `urlpatterns = format_suffix_patterns(urlpatterns)` - add to `urls.py` so you can access the link with `.json` at the end
> Add `format=None` to the view that you want to add a format suffix 

###### Modules

1. `from rest_framework import status`

2. `from rest_framework.decorators import api_view`

3. `from rest_framework.response import Response`

#####