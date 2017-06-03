Instructions for the use of the csvimport django app
====================================================

### Test csvimport

#### Install csvimport in a virtual environment

    $ mkproject -p python3 csvmimport-test
    $ pip3 install django
    $ pip3 install django-csvimport

#### Load csvimport.settings into django-admin.py

Go to the virtual environment directory

    $ cdvirtualenv

Create the django-admin.py file

    cat > bin/django-admin.py << EOF
    #!/usr/bin/env python
    from django.core import management
    import os
    os.environ["DJANGO_SETTINGS_MODULE"] = "csvimport.settings"
    if __name__ == "__main__":
        management.execute_from_command_line()
    EOF

#### Check that the csvimport Country model is empty

    $ django-admin.py shell
    >>> from csvimport.tests.models import Country
    >>> Country.objects.all()
    >>> exit()

#### Import a csv file into the importcsv Country model

Create a superuser account

    $ django-admin.py createsuperuser

Migrate csvimport files and start the server

    $ django-admin.py migrate
    $ django-admin.py runserver

Go to http://127.0.0.1:8000/admin/ and register with your
superuser account

Click on the add button next to csv import

In the model name input field type

    csvimport.Country

Upload the following file

    ~/.virtualenvs/csvimport-test/lib/python3.6/site-pacakages/csvimport/tests/fixtures/countries.csv

Click on save

#### Check that the csvimport Country model is now populated

    $ django-admin.py shell
    >>> from csvimport.tests.models import Country
    >>> Country.objects.all()
    >>> exit()
