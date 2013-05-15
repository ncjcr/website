Instalation
===========

#. Install Python 2.6 or Python 2.7 with headers.
#. Make sure development packages of libxml2 and libxslt are installed.
#. Install Python Imaging Library (PIL).
#. python bootstrap.py -d -c buildout.cfg
#. Depending on whether you want to develop or deploy run:

   * ./bin/buildout

   * ./bin/buildout -c deployment.cfg

Bugs and TODO
=============

- There is no way to set LDAP cache settings using GenericSetup
- "Groups not stored on the server" (LDAP) is not imported with GenericSetup step
