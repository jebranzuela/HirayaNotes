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
        'PASSWORD': 'penguinbercinta',
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

##### Modules

1. `from django.http import HttpResponse` - used to return an Httpresponse from `view`.

