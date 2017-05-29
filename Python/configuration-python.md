Pyhon Configuration
===================

Pylint
------

### Install

* Using pacman (recommended):
  `pacman -S python-pylint` and `pacman -S python2-pyliny'

* Using pip:
  `pip install pylint` and `pip2 install pylint`

Virtualenv and virtualenvwrapper
--------------------------------

### Install

* Using pacman (recommended):
  `pacman -S python-virtualenv`
  `pacman -S python-virtualenvwrapper`

* Using pip:
  `pip install virtualenv`
  `pip install virtualenvwrapper`

Add the following lines to `~/.zshrc`:

    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/projects

And the following line:

* If virtualenvwrapper was installed through pacman:

    source /usr/bin/virtualenvwrapper.sh

* If virtualenvwrapper was installed through pip:

    source /usr/local/bin/virtualenvwrapper.sh

### Useful Commands

* List all virtual environments
  `workon`
* Make a `my_project` folder in $PROJECT_HOME and `cd` to it
  `mkproject my_project`
* Deactivate current environment
  `deactivate`

### Workflow

...

Matplotlib
----------

### Install

* Using pacman (recommended):
  `pacman -S python-matplotlib` and `pacman -S python2-matplotlib`

* Using pip:
  `pip install matplotlib` and `pip2 install matplotlib`
