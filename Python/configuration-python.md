Pyhon Configuration
===================

Install pylint
--------------

`pip install pylint` and `pip2 install pylint`

Install virtualenv and virtualenvwrapper
----------------------------------------

`pip install virtualenv`

`pip install virtualenvwrapper`

Add the following lines to `~/.zshrc`:

    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/projects
    source /usr/local/bin/virtualenvwrapper.sh

### Useful Commands

* List all virtual environments  
  `workon`
* Make a _project_ folder in $PROJECT_HOME and `cd` to it  
  `mkproject _project_`

### Workflow

...
