.. _truth:

Truth Value Testing
===================

.. index::
   statement: if
   statement: while
   pair: truth; value
   pair: Boolean; operations
   single: false

Any object can be tested for truth value, for use in an :keyword:`if` or
:keyword:`while` condition or as operand of the Boolean operations below. The
following values are considered false:

  .. index:: single: None (Built-in object)

* ``None``

  .. index:: single: False (Built-in object)

* ``False``

* zero of any numeric type, for example, ``0``, ``0L``, ``0.0``, ``0j``.

* any empty sequence, for example, ``''``, ``()``, ``[]``.

* any empty mapping, for example, ``{}``.

* instances of user-defined classes, if the class defines a :meth:`__nonzero__`
  or :meth:`__len__` method, when that method returns the integer zero or
  :class:`bool` value ``False``. [1]_

.. index:: single: true

All other values are considered true --- so objects of many types are always
true.

.. index::
   operator: or
   operator: and
   single: False
   single: True

Operations and built-in functions that have a Boolean result always return ``0``
or ``False`` for false and ``1`` or ``True`` for true, unless otherwise stated.
(Important exception: the Boolean operations ``or`` and ``and`` always return
one of their operands.)