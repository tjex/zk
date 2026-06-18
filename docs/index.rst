.. image:: assets/media/zk-black-modern.png
   :align: center
   :width: 300px

.. image:: assets/media/screencast.svg
   :align: center

.. toctree::
   :hidden:
   :titlesonly:

   GitHub <https://github.com/zk-org/zk>
   Neovim Plugin <https://github.com/zk-org/zk-nvim>

   config/index
   notes/index
   tips/index

`zk` is a plain text note-taking assistant. It's built around the power and
design of the Unix command line to make your knowledge generation and managment
process highly interoperable and leveragable. 

It can be used to build and maintain massive Zettelkasten collections,
documentation sites, personal blogs, etc. The contents of this documentation
site itself is written with the help of `zk`.

`zk` itself is not a note writing or editing program itself. It helps you to
manage your note collections and gives you powerful functionality to query
relationships, contents and metadata of the information contained in those
collections. There is a templating system and methods to dynamically inject data
into those templates, depending on what inputs you provide. Such functionality
makes `zk` equally useful for keeping meeting notes, a diary or a devlog at work
or in your projects.

To write your notes, you are free to use any program you like. There are also
plugins for neovim <https://github.com/zk-org/zk-nvim>, emacs
<https://codeberg.org/mcookly/zk-emacs> and VSCode
<https://github.com/zk-org/zk-vscode> that integrate with the functionality of
`zk`, creating a 'frontend/backend' model. `zk` can also work alongside/within
an Obsidian vault.

`zk` also ships with it's own LSP, allowing you to deeply integrate into other
programs or lower level workflows. 

Install as below and then... :doc:`get zettling <tips/getting-started>`!

This site always represents the latest state of the documentation.

Installation
============

Homebrew:

.. code-block:: sh
   
   brew install zk

   # Or, if you want to be on the bleeding edge:
   brew install --HEAD zk


Nix:

.. code-block:: sh

   # Run zk from Nix store without installing it:
   nix run nixpkgs#zk

   # Or, to install it permanently:
   nix-env -iA zk

Alpine Linux:

.. code-block:: sh

   # `zk` is currently available in the `testing` repositories:
   apk add zk

Arch Linux:

You can install `the zk package <https://archlinux.org/packages/extra/x86_64/zk/>`_ from the official repos.

.. code-block:: sh

   sudo pacman -S zk

Build from scratch:

Make sure you have a working `Go 1.24+ installation <https://golang.org/>`_, then clone the repository:

.. code-block:: sh

   git clone https://github.com/zk-org/zk.git
   cd zk

On macOS / Linux:

.. code-block:: sh

   make
   ./zk -h



