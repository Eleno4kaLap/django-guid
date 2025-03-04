.. raw:: html

    <p align="center">
        <h1 align="center">Django GUID</h1>
    </p>
    <p align="center">
      <em>Now with ASGI support!</em>
    </p>

.. raw:: html

    <p align="center">
      <a href="https://pypi.org/pypi/django-guid">
          <img src="https://img.shields.io/pypi/v/django-guid.svg" alt="Package version">
      </a>
      <a href="https://pypi.python.org/pypi/django-guid#downloads">
          <img src="https://img.shields.io/badge/python-3.7+-blue.svg" alt="Downloads">
      </a>
      <a href="https://pypi.python.org/pypi/django-guid">
          <img src="https://img.shields.io/pypi/djversions/django-guid?color=0C4B33&logo=django&logoColor=white&label=django" alt="Django versions">
      </a>
      </a>
      <a href="https://img.shields.io/badge/ASGI-supported-brightgreen.svg">
          <img src="https://img.shields.io/badge/ASGI-supported-brightgreen.svg" alt="ASGI">
      </a>
      <a href="https://img.shields.io/badge/WSGI-supported-brightgreen.svg">
          <img src="https://img.shields.io/badge/WSGI-supported-brightgreen.svg" alt="WSGI">
      </a>
    </p>
    <p align="center">
      <a href="https://django-guid.readthedocs.io/en/latest/?badge=latest">
          <img src="https://readthedocs.org/projects/django-guid/badge/?version=latest" alt="Docs">
      </a>

      <a href="https://codecov.io/gh/snok/django-guid">
          <img src="https://codecov.io/gh/snok/django-guid/branch/master/graph/badge.svg" alt="Codecov">
      </a>

      <a href="https://github.com/psf/black">
          <img src="https://img.shields.io/badge/code%20style-black-000000.svg" alt="Black">
      </a>
      <a href="https://github.com/pre-commit/pre-commit">
          <img src="https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white" alt="Pre-commit">
      </a>
    </p>


--------------


Django GUID attaches a unique correlation ID/request ID to all your log outputs for every request.
In other words, all logs connected to a request now has a unique ID attached to it, making debugging simple.

Which version of Django GUID you should use depends on your Django version and whether you run ``ASGI`` or ``WSGI`` servers.
To determine which Django-GUID version you should use, please see the table below.


+---------------------+--------------------------+
|   Django version    |   Django-GUID version    |
+=====================+==========================+
| 3.1.1 or above      |  3.x.x - ASGI and WSGI   |
+---------------------+--------------------------+
| 3.0.0 - 3.1.0       |  2.x.x - Only WSGI       |
+---------------------+--------------------------+
| 2.2.x               |  2.x.x - Only WSGI       |
+---------------------+--------------------------+

Django GUID >= 3.0.0 uses ``ContextVar`` to store and access the GUID. Previous versions stored the GUID to an object,
making it accessible by using the ID of the current thread. (Version 2 of Django GUID is supported until Django2.2 LTS is over.)

--------------


**Resources**:

* Free software: BSD License
* Documentation: https://django-guid.readthedocs.io
* Homepage: https://github.com/snok/django-guid

--------------


**Examples**

Log output with a GUID:

.. code-block:: flex

    INFO ... [773fa6885e03493498077a273d1b7f2d] project.views This is a DRF view log, and should have a GUID.
    WARNING ... [773fa6885e03493498077a273d1b7f2d] project.services.file Some warning in a function
    INFO ... [0d1c3919e46e4cd2b2f4ac9a187a8ea1] project.views This is a DRF view log, and should have a GUID.
    INFO ... [99d44111e9174c5a9494275aa7f28858] project.views This is a DRF view log, and should have a GUID.
    WARNING ... [0d1c3919e46e4cd2b2f4ac9a187a8ea1] project.services.file Some warning in a function
    WARNING ... [99d44111e9174c5a9494275aa7f28858] project.services.file Some warning in a function


Log output without a GUID:

.. code-block:: flex

    INFO ... project.views This is a DRF view log, and should have a GUID.
    WARNING ... project.services.file Some warning in a function
    INFO ... project.views This is a DRF view log, and should have a GUID.
    INFO ... project.views This is a DRF view log, and should have a GUID.
    WARNING ... project.services.file Some warning in a function
    WARNING ... project.services.file Some warning in a function


See the `documentation <https://django-guid.readthedocs.io>`_ for more examples.

************
Installation
************

Install using pip:

.. code-block:: bash

    pip install django-guid


********
Settings
********

Package settings are added in your ``settings.py``:

.. code-block:: python

    DJANGO_GUID = {
        'GUID_HEADER_NAME': 'Correlation-ID',
        'VALIDATE_GUID': True,
        'RETURN_HEADER': True,
        'EXPOSE_HEADER': True,
        'INTEGRATIONS': [],
        'IGNORE_URLS': [],
        'UUID_LENGTH': 32,
        'LOG_REQUESTS': False
    }



**Optional Parameters**

* :code:`GUID_HEADER_NAME`
        The name of the GUID to look for in a header in an incoming request. Remember that it's case insensitive.

    Default: Correlation-ID

* :code:`VALIDATE_GUID`
        Whether the :code:`GUID_HEADER_NAME` should be validated or not.
        If the GUID sent to through the header is not a valid GUID (:code:`uuid.uuid4`).

    Default: True

* :code:`RETURN_HEADER`
        Whether to return the GUID (Correlation-ID) as a header in the response or not.
        It will have the same name as the :code:`GUID_HEADER_NAME` setting.

    Default: True

* :code:`EXPOSE_HEADER`
        Whether to return :code:`Access-Control-Expose-Headers` for the GUID header if
        :code:`RETURN_HEADER` is :code:`True`, has no effect if :code:`RETURN_HEADER` is :code:`False`.
        This is allows the JavaScript Fetch API to access the header when CORS is enabled.

    Default: True

* :code:`INTEGRATIONS`
        Whether to enable any custom or available integrations with :code:`django_guid`.
        As an example, using :code:`SentryIntegration()` as an integration would set Sentry's :code:`transaction_id` to
        match the GUID used by the middleware.

    Default: []

* :code:`IGNORE_URLS`
        URL endpoints where the middleware will be disabled. You can put your health check endpoints here.

    Default: []

* :code:`UUID_LENGTH`
        Lets you optionally trim the length of the package generated UUIDs.

    Default: 32

* :code:`LOG_REQUESTS`
        Ability to log all requests received by the application, including the user ID (if present)

    Default: False

*************
Configuration
*************

Once settings have set up, add the following to your projects' ``settings.py``:

1. Installed Apps
=================

Add :code:`django_guid` to your :code:`INSTALLED_APPS`:

.. code-block:: python

    INSTALLED_APPS = [
        ...
        'django_guid',
    ]


2. Middleware
=============

Add the :code:`django_guid.middleware.guid_middleware` to your ``MIDDLEWARE``:

.. code-block:: python

    MIDDLEWARE = [
        'django_guid.middleware.guid_middleware',
        ...
     ]


It is recommended that you add the middleware at the top, so that the remaining middleware loggers include the requests GUID.

3. Logging Configuration
========================

Add :code:`django_guid.log_filters.CorrelationId` as a filter in your ``LOGGING`` configuration:

.. code-block:: python

    LOGGING = {
        ...
        'filters': {
            'correlation_id': {
                '()': 'django_guid.log_filters.CorrelationId'
            }
        }
    }

Put that filter in your handler:

.. code-block:: python

    LOGGING = {
        ...
        'handlers': {
            'console': {
                'class': 'logging.StreamHandler',
                'formatter': 'medium',
                'filters': ['correlation_id'],
            }
        }
    }

And make sure to add the new ``correlation_id`` filter to one or all of your formatters:

.. code-block:: python

    LOGGING = {
        ...
        'formatters': {
            'medium': {
                'format': '%(levelname)s %(asctime)s [%(correlation_id)s] %(name)s %(message)s'
            }
        }
    }


If these settings were confusing, please have a look in the demo projects'
`settings.py <https://github.com/snok/django-guid/blob/master/demoproj/settings.py>`_ file for a complete example.

4. Django GUID Logger (Optional)
================================

If you wish to see the Django GUID middleware outputs, you may configure a logger for the module.
Simply add django_guid to your loggers in the project, like in the example below:

.. code-block:: python

    LOGGING = {
        ...
        'loggers': {
            'django_guid': {
                'handlers': ['console', 'logstash'],
                'level': 'WARNING',
                'propagate': False,
            }
        }
    }

This is especially useful when implementing the package, if you plan to pass existing GUIDs to the middleware, as misconfigured GUIDs will not raise exceptions, but will generate warning logs.
