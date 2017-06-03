Recommended steps to set up a project in Django
-----------------------------------------------

#### Install django in a virtual environment

First of all, install virtualenv and virtualenvwrapper.

Create a folder and a virtual environment for your django project

    $ mkproject -p python3 project_folder

Install django

    $ pip3 install django

#### Create your django project

    $ django-admin startproject django_project_name .

#### Hide your secret key

Open your settings.py file and copy the `SECRET_KEY` line

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
    $ workon project_folder
