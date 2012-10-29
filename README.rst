Instalation
===========

Development
-----------

#. Install Python 2.6 with headers, PIL.
#. python2.6 bootstrap.py -d
#. ./bin/buildout

Deployment
----------

#. Install Python 2.6 with headers, PIL.
#. python2.6 bootstrap.py -d
#. ./bin/buildout -c deployment.cfg

Bugs and TODO
=============

- There is no way to set LDAP cache settings using GenericSetup
- "Groups not stored on the server" (LDAP) is not imported with GenericSetup step
