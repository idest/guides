npm Usage
=========

### Install packages locally

#### Cd into <your_project> directory

    $ mkdir <your_projec>
    $ cd <your_project>

#### Create your package.json file


    $ npm init

which will run a command line questionaire that will conclude with the creation of a package.json file. Use the --yes flah if you just want a default package.json file.

Alternatively, you can manually create your package.json file. In `<your_projec>` run the following commands:

    $ cat > package.json <<EOF
    $ {
    $   "name": "demo-app"
    $   "version": "1.0.0"
    $ }
    $ EOF

#### Install packages and save them to the package.json file

If you are building a npm package, and the package you are going to install is **required** to run your package, you should use the _save_ flag:

    $ npm install <package> --save

If you are building a npm package, adn the package you are going to install is **not required** to run your package (e.g. a package for running your tests), you should use the --save-dev flag (these are called devDependencies):

    $ npm install <package> --save-dev

#### Add files to the package.json file

Instead of adding your packages to your package.json file automatically throug `npm install --save` you can add them manually.

Add the following lines to `package.json`:

    "dependencies"" {
      "<packageA>": "<versionA>",
      "<packageB>": "<versionB>"
    }

#### Install packages from your package.json file

From your project folder run:

    $ npm install

#### Uninstall packages and remove them of the package.json file

    $ npm uninstall <package> --save
