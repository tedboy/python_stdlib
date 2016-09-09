.. _specialattrs:

Special Attributes
==================

The implementation adds a few special read-only attributes to several object
types, where they are relevant.  Some of these are not reported by the
:func:`dir` built-in function.


.. attribute:: object.__dict__

   A dictionary or other mapping object used to store an object's (writable)
   attributes.


.. attribute:: object.__methods__

   .. deprecated:: 2.2
      Use the built-in function :func:`dir` to get a list of an object's attributes.
      This attribute is no longer available.


.. attribute:: object.__members__

   .. deprecated:: 2.2
      Use the built-in function :func:`dir` to get a list of an object's attributes.
      This attribute is no longer available.


.. attribute:: instance.__class__

   The class to which a class instance belongs.


.. attribute:: class.__bases__

   The tuple of base classes of a class object.


.. attribute:: definition.__name__

   The name of the class, type, function, method, descriptor, or
   generator instance.


The following attributes are only supported by :term:`new-style class`\ es.

.. attribute:: class.__mro__

   This attribute is a tuple of classes that are considered when looking for
   base classes during method resolution.


.. method:: class.mro()

   This method can be overridden by a metaclass to customize the method
   resolution order for its instances.  It is called at class instantiation, and
   its result is stored in :attr:`~class.__mro__`.


.. method:: class.__subclasses__

   Each new-style class keeps a list of weak references to its immediate
   subclasses.  This method returns a list of all those references still alive.
   Example::

      >>> int.__subclasses__()
      [<type 'bool'>]


.. rubric:: Footnotes

.. [1] Additional information on these special methods may be found in the Python
   Reference Manual (:ref:`customization`).

.. [2] As a consequence, the list ``[1, 2]`` is considered equal to ``[1.0, 2.0]``, and
   similarly for tuples.

.. [3] They must have since the parser can't tell the type of the operands.

.. [4] Cased characters are those with general category property being one of
   "Lu" (Letter, uppercase), "Ll" (Letter, lowercase), or "Lt" (Letter, titlecase).

.. [5] To format only a tuple you should therefore provide a singleton tuple whose only
   element is the tuple to be formatted.

.. [6] The advantage of leaving the newline on is that returning an empty string is
   then an unambiguous EOF indication.  It is also possible (in cases where it
   might matter, for example, if you want to make an exact copy of a file while
   scanning its lines) to tell whether the last line of a file ended in a newline
   or not (yes this happens!).