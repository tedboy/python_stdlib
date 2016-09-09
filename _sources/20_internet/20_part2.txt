

**********************************
20b Internet Protocols and Support
**********************************

.. index::
   single: WWW
   single: Internet
   single: World Wide Web

.. index:: module: socket

The modules described in this chapter implement Internet protocols and  support
for related technology.  They are all implemented in Python. Most of these
modules require the presence of the system-dependent module :mod:`socket`, which
is currently supported on most popular platforms.  Here is an overview:


.. toctree::
   :maxdepth: 1
   :numbered:
   
   telnetlib
   uuid
   urlparse
   socketserver
   basehttpserver
   simplehttpserver
   cgihttpserver
   cookielib
   cookie
   xmlrpclib
   simplexmlrpcserver
   docxmlrpcserver