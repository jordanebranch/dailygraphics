Copyright 2013 NPR.  All rights reserved.  No part of these materials may be reproduced, modified, stored in a retrieval system, or retransmitted, in any form or by any means, electronic, mechanical or otherwise, without prior written permission from NPR.

(Want to use this code? Send an email to nprapps@npr.org!)

Daily Graphics
========================

* [What is this?](#what-is-this)
* [Assumptions](#assumptions)
* [What's in here?](#whats-in-here)
* [Bootstrap the project](#bootstrap-the-project)
* [Hide project secrets](#hide-project-secrets)
* [Adding a new graphic to the project](#adding-a-new-graphic-to-the-project)
* [Run the project](#run-the-project)
* [Run Python tests](#run-python-tests)
* [Run Javascript tests](#run-javascript-tests)
* [Deploy to S3](#deploy-to-s3)
* [Embedding on NPR](#embedding-on-npr)

What is this?
-------------

Daily Graphics is a rig for creating simple Daily Graphics for publishing on NPR.org with responsive iframe embeds.

Assumptions
-----------

The following things are assumed to be true in this documentation.

* You are running OSX.
* You are using Python 2.7. (Probably the version that came OSX.)
* You have [virtualenv](https://pypi.python.org/pypi/virtualenv) and [virtualenvwrapper](https://pypi.python.org/pypi/virtualenvwrapper) installed and working.
* You have NPR's AWS credentials stored as environment variables locally.

For more details on the technology stack used with the app-template, see our [development environment blog post](http://blog.apps.npr.org/2013/06/06/how-to-setup-a-developers-environment.html).

What's in here?
---------------

The project contains the following folders and important files:

* ``templates`` -- HTML ([Jinja2](http://jinja.pocoo.org/docs/)) templates, to be compiled locally.
* ``tests`` -- Python unit tests.
* ``www`` -- Static and compiled assets to be deployed. (a.k.a. "the output")
* ``www/assets`` -- A symlink to an S3 bucket containing binary assets (images, audio).
* ``www/live-data`` -- "Live" data deployed to S3 via cron jobs or other mechanisms. (Not deployed with the rest of the project.)
* ``www/test`` -- Javascript tests and supporting files.
* ``app.py`` -- A [Flask](http://flask.pocoo.org/) app for rendering the project locally.
* ``app_config.py`` -- Global project configuration for scripts, deployment, etc.
* ``fabfile.py`` -- [Fabric](http://docs.fabfile.org/en/latest/) commands automating setup and deployment.
* ``render_utils.py`` -- Code supporting template rendering.
* ``requirements.txt`` -- Python requirements.
* ``static.py`` -- Static Flask views used in both ``app.py`` and ``public_app.py``.

Bootstrap the project
---------------------

```
cd dailygraphics
mkvirtualenv --no-site-packages dailygraphics
pip install -r requirements.txt
```

Hide project secrets
--------------------

Project secrets should **never** be stored in ``app_config.py`` or anywhere else in the repository. They will be leaked to the client if you do. Instead, always store passwords, keys, etc. in environment variables and document that they are needed here in the README.

Adding a new graphic to the project
-------------------------

All of the daily graphics to be put on NPR.org will live in this repo. To add a new graphic, run ```fab add_graphic:NAME_OF_GRAPHIC```.

This will create the folder ```www/graphics/NAME_OF_GRAPHIC```. Within that folder will be a ```child.html``` file.

Create the graphic in that file, and add any of the CSS/JS that you need within that folder.

**Note**: `NAME_OF_GRAPHIC` should be URL-safe, e.g., lowercase and with dashes instead of spaces and no special characters.

Here are some examples:

* Good: my-project-name
* Bad: My-Project-NAME

* Good: my-project-name
* Bad: my project name

* Good: my-wonderful-project
* Bad: my wonderful project!

Run the project
---------------

A flask app is used to run the project locally. It will automatically recompile templates and assets on demand.

```
workon dailygraphics
python app.py
```

Visit ```http://localhost:8000/graphics/NAME_OF_GRAPHIC``` in your browser.


Run Python tests
----------------

Python unit tests are stored in the ``tests`` directory. Run them with ``fab tests``.

Run Javascript tests
--------------------

With the project running, visit [localhost:8000/test/SpecRunner.html](http://localhost:8000/test/SpecRunner.html).

Deploy to S3
------------

```
fab staging master deploy
```

Embedding on NPR
----------------

Deploy the project to production. Visit ```http://apps.npr.org/graphics/NAME_OF_GRAPHIC```, and on that page should be an ```iframe``` with your graphic inside of it, and an embed code below the graphic. Use the embed code in Seamus to make the graphic work on an NPR story page.
