Recommended steps to set up a project in Django
-----------------------------------------------

#### Install django in a virtual environment

First of all, install virtualenv and virtualenvwrapper.

Create a folder and a virtual environment for your django project

    $ mkproject -p python3 <project_folder>

Install django

    $ pip3 install django

#### Create your django project

    $ django-admin startproject <django_project_name> .

#### Hide your secret key

Open your settings.py file and copy the `SECRET_KEY` line

    $ cat django_project_name/settings.py | grep SECRET_KEY

Go to the `bin` directory of your virtual environment

    $ cdvirtualenv
    $ cd bin

Append the following lines to the end of the `activate` script

    ### andestm-web SECRET_KEY
    export SECRET_KEY="<secret_key>"

Finally, replace the `SECRET_KEY` line from your settings.py file, for the
following line:

    SECRET_KEY = os.environ['SECRET_KEY']

Restart your virtual environment

    $ deactivate
    $ workon <project_folder>

#### Apply initial migrations

    $ python manage.py migrate

#### Create super user

    $ python manage.py createsuperuser

#### Create your first app

    $ python manage.py startapp <app_name>

#### Register your app in your django settings

Edit `django_project_name/settings.py` and add the following line to the
INSTALLED_APPS list

    <app_name>.apps.<App_name>Config,

#### Include your app urls into the project's main urls file

Edit `django_project_name/urls.py` and add the following line:

    from django.conf.urls import include

And then add the folliwng line to the `urlpatterns` list:

    url(r'<regex>', include('<app_name>.urls')),

#### Write the urls file for your app, and add the index url to it

Create the `django_project_name/app_name/urls.py` file and add the following
lines:

    from django.conf.urls import url
    from . import views

    app_name = '<app_name>'
    urlpatterns = [
        url(r'^$', views.index, name='index'),
    ]

#### Create a base.html template from which other templates extend

from your `<project_folder>`

    $ mkdir <django_project_name>/templates
    $ touch <django_project_name>/templates/base.html

add the following line to the TEMPLATES section of your settings.py file:

    $ 'DIRS': [os.path.join(BASE_DIR, '<django_project_name>/templates')],
