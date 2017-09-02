Webpack integration for your django project
===========================================

This guide is built after owaislone.org/blog/webpack-plus-reactjs-and-django/

This guide assumes you have npm installed, and you know its basic usage.

### Create the package.json file for your project in your `<django_project_container>` directory (where manage.py is located)

    $ cd <django_project_container>
    $ npm init

### Install webpack and webpack-bundle-tracker as devDependencies

    $ npm install --save-dev webpack webpack-bundle-tracker

### Install babel and babel-loader as devDependencies

This is needed if you plan on using ES6 syntax

    $ npm install --save-dev babel-loader babel-core babel-preset-env

You can optionally _also_ install webpack as a global module to install it to a directory in your $PATH, so you can run it from the command line

    $ npm install webpack -g

### Crete the `webpack.config.js` file

    $ touch webpack.config.js

### Create the entry js file required by webpack

    $ mkdir -p assets/js
    $ touch assets/js/index.js

### Create the `webpack.config.js` file

    $ touch webpack.config.js

Add the following lines to `webpack.config.js`:

    var webpack = require('webpack');
    var BundleTracker = require('webpack-bundle-tracker');

    module.exports = {
      context: __dirname,

      entry: './assets/js/index',

      output: {
        path: __dirname + "/assets/bundles",
        filename: "[name]-[hash].js"
      },

      plugins: [
        new BundleTracker({filename: './webpack-stats.json'})
      ],
    };

If you plan on using ES6 syntax, also add the following lines (before the last `};`)

    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['env']
            }
          }
        }
      ]
    }

### Compile a bundle

If you have installed webpack globally:

    $ webpack

otherwise:

    $ ./node-modules/.bin/webpack --config webpack.config.js

This will take all the modules imported into `./assets/js/index.js` and bundle them to a file located in /assets/bundles

### Install django-webpack-loader python's package

    $ pip install django-webpack-loader

### Add django-webpack-loader to your django settings, and configure it

Add the following lines to `settings.py`:

    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'assets'),
    ]

    WEBPACK_LOADER = {
        'DEFAULT': {
             'BUNDLE_DIR_NAME': 'bundles/',
             'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
        }
    }

    INSTALLED_APPS = [
        'webpack_loader',
        ...
     ]

### Add webpack's bundled files to your templates

Use this template commands to load your bundled files.

First load the render_bundle function (do this at the top of your template):

    {% load render_bundle from webpack_loader %}

Then insert your `<script>` and `<link>` tags using the following command:

    {% render_bundle 'file_name' %}
