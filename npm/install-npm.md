npm Installation
================

#### Install node.js

* On MacOSX:

  Download the node.js macOS Installer .pkg from: http://nodejs.org/en/download/ and install it.

* On Archlinux:

      $ sudo pacman -S nodejs
      $ sudo pacman -S npm

#### Fixing npm permissions

Change the location of the npm global packages

    $ mkdir ~/.npm-global
    $ npm config set prefix $HOME/.npm-global

Add your .npm-global/bin folder to your PATH

Add the following line to your .bash_profile (MacOSX) or .bashrc (Linux) file:

    export PATH=$HOME/.npm-global:$PATH

#### Update npm

    $ npm install npm -g

#### Fix symlink to npm

This fix is needed if you updated npm via `npm update -g npm`, after running `npm config set prefix $HOME/.npm-global` as this will replace your current npm-cli.js location.

You will need to change the symlink in `/usr/local/bin/npm` currently pointing to `/usr/local/lib/node_modules/npm/bin/npm-cli.js` for a symlink pointing to `~/.npm-global/lib/node-modules/npm/bin/npm-cli.js`
