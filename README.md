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
..* This will be used mainly to edit `location /media` and `location/static`. You can change the file path for `media` and `static` files as you wish. Just remember to put all the assets of your project here.

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