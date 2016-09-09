.. _typesother:

Other Built-in Types
====================

The interpreter supports several other kinds of objects. Most of these support
only one or two operations.


.. _typesmodules:

Modules
-------

The only special operation on a module is attribute access: ``m.name``, where
*m* is a module and *name* accesses a name defined in *m*'s symbol table.
Module attributes can be assigned to.  (Note that the :keyword:`import`
statement is not, strictly speaking, an operation on a module object; ``import
foo`` does not require a module object named *foo* to exist, rather it requires
an (external) *definition* for a module named *foo* somewhere.)

A special attribute of every module is :attr:`~object.__dict__`. This is the
dictionary containing the module's symbol table. Modifying this dictionary will
actually change the module's symbol table, but direct assignment to the
:attr:`~object.__dict__` attribute is not possible (you can write
``m.__dict__['a'] = 1``, which defines ``m.a`` to be ``1``, but you can't write
``m.__dict__ = {}``).  Modifying :attr:`~object.__dict__` directly is
not recommended.

Modules built into the interpreter are written like this: ``<module 'sys'
(built-in)>``.  If loaded from a file, they are written as ``<module 'os' from
'/usr/local/lib/pythonX.Y/os.pyc'>``.


.. _typesobjects:

Classes and Class Instances
---------------------------

See :ref:`objects` and :ref:`class` for these.


.. _typesfunctions:

Functions
---------

Function objects are created by function definitions.  The only operation on a
function object is to call it: ``func(argument-list)``.

There are really two flavors of function objects: built-in functions and
user-defined functions.  Both support the same operation (to call the function),
but the implementation is different, hence the different object types.

See :ref:`function` for more information.


.. _typesmethods:

Methods
-------

.. index:: object: method

Methods are functions that are called using the attribute notation. There are
two flavors: built-in methods (such as :meth:`append` on lists) and class
instance methods.  Built-in methods are described with the types that support
them.

The implementation adds two special read-only attributes to class instance
methods: ``m.im_self`` is the object on which the method operates, and
``m.im_func`` is the function implementing the method.  Calling ``m(arg-1,
arg-2, ..., arg-n)`` is completely equivalent to calling ``m.im_func(m.im_self,
arg-1, arg-2, ..., arg-n)``.

Class instance methods are either *bound* or *unbound*, referring to whether the
method was accessed through an instance or a class, respectively.  When a method
is unbound, its ``im_self`` attribute will be ``None`` and if called, an
explicit ``self`` object must be passed as the first argument.  In this case,
``self`` must be an instance of the unbound method's class (or a subclass of
that class), otherwise a :exc:`TypeError` is raised.

Like function objects, methods objects support getting arbitrary attributes.
However, since method attributes are actually stored on the underlying function
object (``meth.im_func``), setting method attributes on either bound or unbound
methods is disallowed.  Attempting to set an attribute on a method results in
an :exc:`AttributeError` being raised.  In order to set a method attribute, you
need to explicitly set it on the underlying function object::

   >>> class C:
   ...     def method(self):
   ...         pass
   ...
   >>> c = C()
   >>> c.method.whoami = 'my name is method'  # can't set on the method
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   AttributeError: 'instancemethod' object has no attribute 'whoami'
   >>> c.method.im_func.whoami = 'my name is method'
   >>> c.method.whoami
   'my name is method'


See :ref:`types` for more information.


.. index:: object; code, code object

.. _bltin-code-objects:

Code Objects
------------

.. index::
   builtin: compile
   single: func_code (function object attribute)

Code objects are used by the implementation to represent "pseudo-compiled"
executable Python code such as a function body. They differ from function
objects because they don't contain a reference to their global execution
environment.  Code objects are returned by the built-in :func:`compile` function
and can be extracted from function objects through their :attr:`func_code`
attribute. See also the :mod:`code` module.

.. index::
   statement: exec
   builtin: eval

A code object can be executed or evaluated by passing it (instead of a source
string) to the :keyword:`exec` statement or the built-in :func:`eval` function.

See :ref:`types` for more information.


.. _bltin-type-objects:

Type Objects
------------

.. index::
   builtin: type
   module: types

Type objects represent the various object types.  An object's type is accessed
by the built-in function :func:`type`.  There are no special operations on
types.  The standard module :mod:`types` defines names for all standard built-in
types.

Types are written like this: ``<type 'int'>``.


.. _bltin-null-object:

The Null Object
---------------

This object is returned by functions that don't explicitly return a value.  It
supports no special operations.  There is exactly one null object, named
``None`` (a built-in name).

It is written as ``None``.


.. _bltin-ellipsis-object:

The Ellipsis Object
-------------------

This object is used by extended slice notation (see :ref:`slicings`).  It
supports no special operations.  There is exactly one ellipsis object, named
:const:`Ellipsis` (a built-in name).

It is written as ``Ellipsis``.  When in a subscript, it can also be written as
``...``, for example ``seq[...]``.


The NotImplemented Object
-------------------------

This object is returned from comparisons and binary operations when they are
asked to operate on types they don't support. See :ref:`comparisons` for more
information.

It is written as ``NotImplemented``.


Boolean Values
--------------

Boolean values are the two constant objects ``False`` and ``True``.  They are
used to represent truth values (although other values can also be considered
false or true).  In numeric contexts (for example when used as the argument to
an arithmetic operator), they behave like the integers 0 and 1, respectively.
The built-in function :func:`bool` can be used to convert any value to a
Boolean, if the value can be interpreted as a truth value (see section
:ref:`truth` above).

.. index::
   single: False
   single: True
   pair: Boolean; values

They are written as ``False`` and ``True``, respectively.


.. _typesinternal:

Internal Objects
----------------

See :ref:`types` for this information.  It describes stack frame objects,
traceback objects, and slice objects.