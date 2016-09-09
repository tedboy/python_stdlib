.. _stdcomparisons:

Comparisons
===========

.. index::
   pair: chaining; comparisons
   pair: operator; comparison
   operator: ==
   operator: <
   operator: <=
   operator: >
   operator: >=
   operator: !=
   operator: is
   operator: is not

Comparison operations are supported by all objects.  They all have the same
priority (which is higher than that of the Boolean operations). Comparisons can
be chained arbitrarily; for example, ``x < y <= z`` is equivalent to ``x < y and
y <= z``, except that *y* is evaluated only once (but in both cases *z* is not
evaluated at all when ``x < y`` is found to be false).

This table summarizes the comparison operations:

+------------+-------------------------+-------+
| Operation  | Meaning                 | Notes |
+============+=========================+=======+
| ``<``      | strictly less than      |       |
+------------+-------------------------+-------+
| ``<=``     | less than or equal      |       |
+------------+-------------------------+-------+
| ``>``      | strictly greater than   |       |
+------------+-------------------------+-------+
| ``>=``     | greater than or equal   |       |
+------------+-------------------------+-------+
| ``==``     | equal                   |       |
+------------+-------------------------+-------+
| ``!=``     | not equal               | \(1)  |
+------------+-------------------------+-------+
| ``is``     | object identity         |       |
+------------+-------------------------+-------+
| ``is not`` | negated object identity |       |
+------------+-------------------------+-------+

Notes:

(1)
    ``!=`` can also be written ``<>``, but this is an obsolete usage
    kept for backwards compatibility only. New code should always use
    ``!=``.

.. index::
   pair: object; numeric
   pair: objects; comparing

Objects of different types, except different numeric types and different string
types, never compare equal; such objects are ordered consistently but
arbitrarily (so that sorting a heterogeneous array yields a consistent result).
Furthermore, some types (for example, file objects) support only a degenerate
notion of comparison where any two objects of that type are unequal.  Again,
such objects are ordered arbitrarily but consistently. The ``<``, ``<=``, ``>``
and ``>=`` operators will raise a :exc:`TypeError` exception when any operand is
a complex number.

.. index::
   single: __cmp__() (instance method)
   single: __eq__() (instance method)
   single: __ne__() (instance method)
   single: __lt__() (instance method)
   single: __le__() (instance method)
   single: __gt__() (instance method)
   single: __ge__() (instance method)

Non-identical instances of a class normally compare as non-equal unless the
class defines the :meth:`__eq__` method or the :meth:`__cmp__` method.

Instances of a class cannot be ordered with respect to other instances of the
same class, or other types of object, unless the class defines either enough of
the rich comparison methods (:meth:`__lt__`, :meth:`__le__`, :meth:`__gt__`, and
:meth:`__ge__`) or the :meth:`__cmp__` method.

.. impl-detail::

   Objects of different types except numbers are ordered by their type names;
   objects of the same types that don't support proper comparison are ordered by
   their address.

.. index::
   operator: in
   operator: not in

Two more operations with the same syntactic priority, ``in`` and ``not in``, are
supported only by sequence types (below).