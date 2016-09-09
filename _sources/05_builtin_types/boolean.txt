.. _boolean:

Boolean Operations --- :keyword:`and`, :keyword:`or`, :keyword:`not`
====================================================================

.. index:: pair: Boolean; operations

These are the Boolean operations, ordered by ascending priority:

+-------------+---------------------------------+-------+
| Operation   | Result                          | Notes |
+=============+=================================+=======+
| ``x or y``  | if *x* is false, then *y*, else | \(1)  |
|             | *x*                             |       |
+-------------+---------------------------------+-------+
| ``x and y`` | if *x* is false, then *x*, else | \(2)  |
|             | *y*                             |       |
+-------------+---------------------------------+-------+
| ``not x``   | if *x* is false, then ``True``, | \(3)  |
|             | else ``False``                  |       |
+-------------+---------------------------------+-------+

.. index::
   operator: and
   operator: or
   operator: not

Notes:

(1)
   This is a short-circuit operator, so it only evaluates the second
   argument if the first one is :const:`False`.

(2)
   This is a short-circuit operator, so it only evaluates the second
   argument if the first one is :const:`True`.

(3)
   ``not`` has a lower priority than non-Boolean operators, so ``not a == b`` is
   interpreted as ``not (a == b)``, and ``a == not b`` is a syntax error.