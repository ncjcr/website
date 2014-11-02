=======================
New College JCR website
=======================

This project is The Oxford University `New College JCR website
<http://jcr.new.ox.ac.uk>`_ buildout.


Infrastructure
==============

The website itself is based on `Plone <http://plone.org/>`_,
well-known `Python <http://python.org/>`_ CMS. The infrastructure::

                         +---------+
                         |         |
                         | Apache  |
                         |         |
                         +----+----+
                              |
                              |
                         +----+----+
                         |         |
                         | Varnish |
                         |         |
                         +----+----+
                              |
                              |
                         +----+----+
                         |         |
                         | HAProxy |
                         |         |
                         +----+----+
                              |
                              |
                    +---------+--------+
                    |                  |
                    |                  |
               +----+-----+      +-----+----+
               |          |      |          |
               |   Zope   |      |   Zope   |
               | Instance | .... | Instance |
               |    1     |      |    N     |
               |          |      |          |
               +----+-----+      +-----+----+
                    |                  |
                    |                  |
                    +---------+--------+
                              |
                              |
                    +---------+--------+
                    |                  |
                    |                  |
              +-----+-----+       +----+----+
              |           |       |         |
              | Memcached |       | Shared  |
              |           |       |  BLOB   |
              +-----+-----+       | Storage |
                    |             |         |
                    |             +---------+
                +---+---+
                |       |
                | MySQL |
                |       |
                +-------+


Backups
=======

Deployment buildout installs crontab entry which backups
RelStorage database and BLOBs regularly. You can run the backup
yourself by typing::

    $ ./bin/backup

Backups are stored in ``var/backups``.

**Remember to backup** before doing any complicated task.

.. note:: It is advisable to back up the buildout once per
          deployment anyway, including the cache of downloaded
          eggs and extended files, in case any downloaded files
          cannot be downloaded again in the future.


BLOBs
-----

BLOBs are by default backed up incrementaly, so if you need a snapshot run::

    $ ./bin/blobbackup-snapshot

BLOBs can be restored by calling either::

    $ ./bin/blobbackup-restore

or::

    $ ./bin/blobbackup-snapshotrestore

Read more on the `collective.recipe.backup site
<https://pypi.python.org/pypi/collective.recipe.backup>`_.


RelStorage
----------

``./bin/relbackup`` script is a wrapper for ``mysqldump`` command,
which additionally uses ``bzip2`` to compress the resulting SQL dump.


Bootstraping
============

#. Install Python 2.6 with headers.

   .. note:: It is advised to run and develop with the same Python
             version as on the deployment server. At the time of
             writing ``jcrweb`` server uses Python 2.6.

   For SLES 11 it would be::

       $ zypper install python python-develop

   and for Debian (7)::

       $ apt-get install python2.6 python2.6-dev

#. Make sure development packages of

   - libxml
   - libxslt
   - libldap2
   - libsasl2
   - libjpeg

   are installed in the system as they are required by some buildout
   parts to compile necessary libraries.

   For Debian this would be appropriately the following packages:

   - libxml2-dev
   - libxslt-dev
   - libldap2-dev
   - libsasl2-dev
   - libjpeg-dev

#. (Deployment) Make sure `mysql_config` binary, necessary for
   MySQL-python, is available on $PATH. For SLES 11::

     $ zypper install libmysqlclient-devel

#. (Deployment) Install rsync (for collective.recipe.backup).

#. Install Python Imaging Library (PIL) (?)

#. Create new `virtualenv <https://pypi.python.org/pypi/virtualenv>`_
   and upgrade setuptools::

     $ virtualenv --no-site-packages -p python2.6 venv
     $ ./venv/bin/pip install setuptools --upgrade

   This is due to numerous problems of colliding system packages and
   what buildout requires, e.g. if setuptoools<0.7 is installed
   system-wide, bootstrap fill fail. Similarily, on jcrweb, bootratrap
   2.2.4 fails because of collision with system-wide
   argparse. `--no-site-packages` will save you from problems, trust
   me!

#. ``$ ./venv/bin/python2.6 bootstrap.py``


Deployment
----------

#. Make sure ``buildout.d/secrets/jcr.new.ox.ac.uk.cfg`` exists
   and contains appropriate usernames/passwords and other secrets.

#. Run buildout::

     $ ./bin/buildout -c jcr.new.ox.ac.uk.cfg

#. Start Supervisor::

     $ ./bin/supervisord

   and control it using the provided tool::

     $ ./bin/supervisorctl

   Read more in the `Supervisor documentation <http://supervisord.org/>`_.

#. Remember to rerun the buildout and restart appropriate processes
   every time a change to the buildout config is made.

#. Reload Apache and firewall as necessary. For SLES::

     $ rcapache2 reload
     $ rcSuSEfirewall2 reload


Testing
-------

The instructions for testing are the same as for deployment,
but update ``buildout.d/secrets/jcrtest.new.ox.ac.uk.cfg`` and
run ``$ ./bin/buildout -c jcrtest.new.ox.ac.uk.cfg`` instead.


Development
-----------

#. Run buildout::

       $ ./bin/buildout -c development.cfg

#. Start Zope instance::

       $ ./bin/instance fg

#. Remember to rerun the buildout every time a change is made.

.. note:: There is a great book called "Professional Plone 4 development"
          which, given you have an SSO access, you can `read online
          <http://www.ebrary.com/landing/site/bodleian/index-bodleian.jsp?Docid=10496813>`_.
          The buildout for this website is based on supplementary
          `source code <https://github.com/optilude/optilux/tree/chapter-18>`_
          from this book.
