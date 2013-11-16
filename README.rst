=======================
New College JCR website
=======================

This project is The Oxford University `New College JCR website
<http://jcr.new.ox.ac.uk>` buildout.


Infrastructure
==============

The website itself is based on `Plone <http://plone.org/>`,
well-known `Python <http://python.org/>` CMS.


Bootstraping
============

#. Install Python 2.6 or Python 2.7 with headers.

   .. warning:: It is better to develop with the same Python version
                as on the deployment server. At the time of writing
                jcrweb server uses Python 2.6.

                For SLES 11 it would be::

                    zypper install python python-develop

                and for Debian (7)::

                    aptitude install python2.6 python2.6-dev

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

#. Install Python Imaging Library (PIL) (?)

#. Install rsync (for collective.recipe.backup).

#. python bootstrap.py

   ..note:: If you are using setuptool<0.7 wou will have to create
            a virtualenviroment, upgrade setuptools and use venv's
            Python binary::

                virtualenv --no-site-packages -p python2.6 venv
                ./venv/bin/pip install setuptools --upgrade
                ./venv/bin/python bootstrap.py

Deployment
----------

Run buildout (every time a change is made)::

    ./bin/buildout -c deployment.cfg

and start the Supervisord::

    ./bin/supervisord


Development
-----------

#. You should create development.cfg buildout config
   which extends buildout.cfg but sets passwords appropriately.

#. Run buildout::

       ./bin/buildout -c development.cfg

#. Start Zope instance:

       ./bin/instance fg

#. Remember to rerun the buildout every time a change is made.

.. note:: There is a great book called "Professional Plone 4 development"
          which you can read online (given you have an SSO access) at
          http://www.ebrary.com/landing/site/bodleian/index-bodleian.jsp?Docid=10496813.
          The buildout for this website is based on supplementary
          `source code <https://github.com/optilude/optilux/tree/chapter-18>`
          from this book.
