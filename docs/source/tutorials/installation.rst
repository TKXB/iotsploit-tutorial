Installation Guide
=================

This guide will help you set up and run the IoTSploit project on your local machine. Follow these steps to get started.

Prerequisites
------------

Before beginning the installation, ensure your system meets the following requirements:

* Python 3.8 or higher
* Docker (for Redis service)
* Git

Detailed Installation Steps
-------------------------

1. Clone Repository and Switch to Development Branch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   git fetch
   git checkout -b dev origin/dev

2. Set Up Redis Service
~~~~~~~~~~~~~~~~~~~~

Ensure Docker is installed, then run the following commands:

.. code-block:: bash

   docker pull redis
   docker run --name sat-redis -p 6379:6379 -d redis:latest

3. Install and Configure Poetry
~~~~~~~~~~~~~~~~~~~~~~~~~~~

We use Poetry for dependency management. Follow these steps to install and configure:

.. code-block:: bash

   pip install poetry
   poetry lock        # This may take 10-20 minutes
   poetry install     # This may take 10-20 minutes
   poetry shell

.. note::
   The Poetry lock and install processes might take some time. Please be patient.

4. Initialize Django Database
~~~~~~~~~~~~~~~~~~~~~~~~~

Run the following commands to set up the database:

.. code-block:: bash

   python manage.py makemigrations
   python manage.py makemigrations sat_toolkit
   python manage.py migrate

5. Start the Application
~~~~~~~~~~~~~~~~~~~~

Launch the application using:

.. code-block:: bash

   python console.py

Verifying Installation
--------------------

After installation, verify that everything is working correctly:

1. Ensure Redis service is running:

   .. code-block:: bash

      docker ps | grep redis

2. Check if IoTSploit Shell is working:

   * Run ``python console.py``
   * Type ``help`` in the shell to see available commands
   * Try running the ``device_info`` command

Common Issues
-----------

If you encounter problems during installation, here are solutions to common issues:

Poetry Issues
~~~~~~~~~~~

- If Poetry installation fails, try upgrading Poetry using pip:

  .. code-block:: bash

     pip install --upgrade poetry

Redis Connection Issues
~~~~~~~~~~~~~~~~~~~~

- If you can't connect to Redis, check if the Docker container is running:

  .. code-block:: bash

     docker ps | grep redis

- If the container isn't running, restart it:

  .. code-block:: bash

     docker start sat-redis

Database Migration Issues
~~~~~~~~~~~~~~~~~~~~~~

- If you encounter database migration errors, try resetting migrations:

  .. code-block:: bash

     python manage.py migrate --fake
     python manage.py migrate --fake-initial

Next Steps
---------

After completing the installation, you can:

* Check out :doc:`/tutorials/basic_usage` to learn basic usage
* Read :doc:`/tutorials/plugin_development` to learn about plugin development
* Refer to :doc:`/api` for API documentation 