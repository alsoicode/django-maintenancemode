This project is no longer maintained and has been moved to: https://github.com/alsoicode/django-maintenancemode-2

======================
django-maintenancemode
======================

django-maintenancemode is a middleware that allows you to temporary shutdown
your site for maintenance work.

Logged in users having staff credentials can still fully use
the site as can users visiting the site from an ip address defined in
Django's `INTERNAL_IPS`.

This fork moves the maintenance mode property and ignored urls out of settings.py
and into your database.

django-maintenancemode works the same way as handling 404 or 500 errors in
Django work. It adds a handler503 which you can override in your main urls.py
or you can add a 503.html to your templates directory.

Requirements
============
django.contrib.sites

Sites must have at least one domain to work properly.


Installation
============

* Download django-maintenancemode from https://github.com/btaylordesign/django-maintenancemode
* Install using: `python setup.py install`
* In your Django settings file add maintenancemode to your `MIDDLEWARE_CLASSES`.
  Make sure it comes after Django's AuthenticationMiddleware. Like so::

   MIDDLEWARE_CLASSES = (
       'django.middleware.common.CommonMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'django.contrib.auth.middleware.AuthenticationMiddleware',
       'django.middleware.doc.XViewMiddleware',
   
       'maintenancemode.middleware.MaintenanceModeMiddleware',
   )
   
* Add ``maintenancemode`` to your `INSTALLED_APPS`.
   
* Run manage.py syncdb to create the necessary tables.

* Adding the middleware and running your site creates the necessary records in the database
  to endable/disbale maintenance mode and ignored URL patterns.


Configuration
=============

``MAINTENANCE MODE``
------------------
Maintenance mode will create a database record per site, read from the domains you have in the
Sites app. There is a boolean property on each Maintenance model, "is_being_performed" that takes
the place of putting the site into "maintnenace mode" from settings.py

``MAINTENANCE IGNORE URLS``
---------------------------
Patterns to ignore are registered as an inline model for each maintenance record created when the
site is first run. Patterns should begin with a forward slash: /, but can end any way you'd like.


Some observations:

* If user is logged in and staff member, the maintenance page is
  not displayed.

* If user's ip is in INTERNAL_IPS, the maintenance page is
  not displayed.
