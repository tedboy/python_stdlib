.. _typesnumeric:

Numeric Types --- :class:`int`, :class:`float`, :class:`long`, :class:`complex`
===============================================================================

.. index::
   object: numeric
   object: Boolean
   object: integer
   object: long integer
   object: floating point
   object: complex number
   pair: C; language

There are four distinct numeric types: :dfn:`plain integers`, :dfn:`long
integers`, :dfn:`floating point numbers`, and :dfn:`complex numbers`. In
addition, Booleans are a subtype of plain integers. Plain integers (also just
called :dfn:`integers`) are implemented using :c:type:`long` in C, which gives
them at least 32 bits of precision (``sys.maxint`` is always set to the maximum
plain integer value for the current platform, the minimum value is
``-sys.maxint - 1``).  Long integers have unlimited precision.  Floating point
numbers are usually implemented using :c:type:`double` in C; information about
the precision and internal representation of floating point numbers for the
machine on which your program is running is available in
:data:`sys.float_info`.  Complex numbers have a real and imaginary part, which
are each a floating point number.  To extract these parts from a complex number
*z*, use ``z.real`` and ``z.imag``. (The standard library includes additional
numeric types, :mod:`fractions` that hold rationals, and :mod:`decimal` that
hold floating-point numbers with user-definable precision.)

.. index::
   pair: numeric; literals
   pair: integer; literals
   triple: long; integer; literals
   pair: floating point; literals
   pair: complex number; literals
   pair: hexadecimal; literals
   pair: octal; literals

Numbers are created by numeric literals or as the result of built-in functions
and operators.  Unadorned integer literals (including binary, hex, and octal
numbers) yield plain integers unless the value they denote is too large to be
represented as a plain integer, in which case they yield a long integer.
Integer literals with an ``'L'`` or ``'l'`` suffix yield long integers (``'L'``
is preferred because ``1l`` looks too much like eleven!).  Numeric literals
containing a decimal point or an exponent sign yield floating point numbers.
Appending ``'j'`` or ``'J'`` to a numeric literal yields a complex number with a
zero real part. A complex numeric literal is the sum of a real and an imaginary
part.

.. index::
   single: arithmetic
   builtin: int
   builtin: long
   builtin: float
   builtin: complex
   operator: +
   operator: -
   operator: *
   operator: /
   operator: //
   operator: %
   operator: **

Python fully supports mixed arithmetic: when a binary arithmetic operator has
operands of different numeric types, the operand with the "narrower" type is
widened to that of the other, where plain integer is narrower than long integer
is narrower than floating point is narrower than complex. Comparisons between
numbers of mixed type use the same rule. [2]_ The constructors :func:`int`,
:func:`long`, :func:`float`, and :func:`complex` can be used to produce numbers
of a specific type.

All built-in numeric types support the following operations. See
:ref:`power` and later sections for the operators' priorities.

+--------------------+---------------------------------+--------+
| Operation          | Result                          | Notes  |
+====================+=================================+========+
| ``x + y``          | sum of *x* and *y*              |        |
+--------------------+---------------------------------+--------+
| ``x - y``          | difference of *x* and *y*       |        |
+--------------------+---------------------------------+--------+
| ``x * y``          | product of *x* and *y*          |        |
+--------------------+---------------------------------+--------+
| ``x / y``          | quotient of *x* and *y*         | \(1)   |
+--------------------+---------------------------------+--------+
| ``x // y``         | (floored) quotient of *x* and   | (4)(5) |
|                    | *y*                             |        |
+--------------------+---------------------------------+--------+
| ``x % y``          | remainder of ``x / y``          | \(4)   |
+--------------------+---------------------------------+--------+
| ``-x``             | *x* negated                     |        |
+--------------------+---------------------------------+--------+
| ``+x``             | *x* unchanged                   |        |
+--------------------+---------------------------------+--------+
| ``abs(x)``         | absolute value or magnitude of  | \(3)   |
|                    | *x*                             |        |
+--------------------+---------------------------------+--------+
| ``int(x)``         | *x* converted to integer        | \(2)   |
+--------------------+---------------------------------+--------+
| ``long(x)``        | *x* converted to long integer   | \(2)   |
+--------------------+---------------------------------+--------+
| ``float(x)``       | *x* converted to floating point | \(6)   |
+--------------------+---------------------------------+--------+
| ``complex(re,im)`` | a complex number with real part |        |
|                    | *re*, imaginary part *im*.      |        |
|                    | *im* defaults to zero.          |        |
+--------------------+---------------------------------+--------+
| ``c.conjugate()``  | conjugate of the complex number |        |
|                    | *c*. (Identity on real numbers) |        |
+--------------------+---------------------------------+--------+
| ``divmod(x, y)``   | the pair ``(x // y, x % y)``    | (3)(4) |
+--------------------+---------------------------------+--------+
| ``pow(x, y)``      | *x* to the power *y*            | (3)(7) |
+--------------------+---------------------------------+--------+
| ``x ** y``         | *x* to the power *y*            | \(7)   |
+--------------------+---------------------------------+--------+

.. index::
   triple: operations on; numeric; types
   single: conjugate() (complex number method)

Notes:

(1)
   .. index::
      pair: integer; division
      triple: long; integer; division

   For (plain or long) integer division, the result is an integer. The result is
   always rounded towards minus infinity: 1/2 is 0, (-1)/2 is -1, 1/(-2) is -1, and
   (-1)/(-2) is 0.  Note that the result is a long integer if either operand is a
   long integer, regardless of the numeric value.

(2)
   .. index::
      module: math
      single: floor() (in module math)
      single: ceil() (in module math)
      single: trunc() (in module math)
      pair: numeric; conversions

   Conversion from floats using :func:`int` or :func:`long` truncates toward
   zero like the related function, :func:`math.trunc`.  Use the function
   :func:`math.floor` to round downward and :func:`math.ceil` to round
   upward.

(3)
   See :ref:`built-in-funcs` for a full description.

(4)
   .. deprecated:: 2.3
      The floor division operator, the modulo operator, and the :func:`divmod`
      function are no longer defined for complex numbers.  Instead, convert to
      a floating point number using the :func:`abs` function if appropriate.

(5)
   Also referred to as integer division.  The resultant value is a whole integer,
   though the result's type is not necessarily int.

(6)
   float also accepts the strings "nan" and "inf" with an optional prefix "+"
   or "-" for Not a Number (NaN) and positive or negative infinity.

   .. versionadded:: 2.6

(7)
   Python defines ``pow(0, 0)`` and ``0 ** 0`` to be ``1``, as is common for
   programming languages.

All :class:`numbers.Real` types (:class:`int`, :class:`long`, and
:class:`float`) also include the following operations:

+--------------------+---------------------------------------------+
| Operation          | Result                                      |
+====================+=============================================+
| :func:`math.trunc(\| *x* truncated to :class:`~numbers.Integral` |
| x) <math.trunc>`   |                                             |
+--------------------+---------------------------------------------+
| :func:`round(x[,   | *x* rounded to *n* digits,                  |
| n]) <round>`       | rounding ties away from zero. If *n*        |
|                    | is omitted, it defaults to 0.               |
+--------------------+---------------------------------------------+
| :func:`math.floor(\| the greatest integer as a float <= *x*      |
| x) <math.floor>`   |                                             |
+--------------------+---------------------------------------------+
| :func:`math.ceil(x)| the least integer as a float >= *x*         |
| <math.ceil>`       |                                             |
+--------------------+---------------------------------------------+

.. XXXJH exceptions: overflow (when? what operations?) zerodivision


.. _bitstring-ops:

Bitwise Operations on Integer Types
--------------------------------------

.. index::
   triple: operations on; integer; types
   pair: bitwise; operations
   pair: shifting; operations
   pair: masking; operations
   operator: ^
   operator: &
   operator: <<
   operator: >>

Bitwise operations only make sense for integers.  Negative numbers are treated
as their 2's complement value (this assumes a sufficiently large number of bits
that no overflow occurs during the operation).

The priorities of the binary bitwise operations are all lower than the numeric
operations and higher than the comparisons; the unary operation ``~`` has the
same priority as the other unary numeric operations (``+`` and ``-``).

This table lists the bitwise operations sorted in ascending priority:

+------------+--------------------------------+----------+
| Operation  | Result                         | Notes    |
+============+================================+==========+
| ``x | y``  | bitwise :dfn:`or` of *x* and   |          |
|            | *y*                            |          |
+------------+--------------------------------+----------+
| ``x ^ y``  | bitwise :dfn:`exclusive or` of |          |
|            | *x* and *y*                    |          |
+------------+--------------------------------+----------+
| ``x & y``  | bitwise :dfn:`and` of *x* and  |          |
|            | *y*                            |          |
+------------+--------------------------------+----------+
| ``x << n`` | *x* shifted left by *n* bits   | (1)(2)   |
+------------+--------------------------------+----------+
| ``x >> n`` | *x* shifted right by *n* bits  | (1)(3)   |
+------------+--------------------------------+----------+
| ``~x``     | the bits of *x* inverted       |          |
+------------+--------------------------------+----------+

Notes:

(1)
   Negative shift counts are illegal and cause a :exc:`ValueError` to be raised.

(2)
   A left shift by *n* bits is equivalent to multiplication by ``pow(2, n)``.  A
   long integer is returned if the result exceeds the range of plain integers.

(3)
   A right shift by *n* bits is equivalent to division by ``pow(2, n)``.


Additional Methods on Integer Types
-----------------------------------

The integer types implement the :class:`numbers.Integral` :term:`abstract base
class`. In addition, they provide one more method:

.. method:: int.bit_length()
.. method:: long.bit_length()

    Return the number of bits necessary to represent an integer in binary,
    excluding the sign and leading zeros::

        >>> n = -37
        >>> bin(n)
        '-0b100101'
        >>> n.bit_length()
        6

    More precisely, if ``x`` is nonzero, then ``x.bit_length()`` is the
    unique positive integer ``k`` such that ``2**(k-1) <= abs(x) < 2**k``.
    Equivalently, when ``abs(x)`` is small enough to have a correctly
    rounded logarithm, then ``k = 1 + int(log(abs(x), 2))``.
    If ``x`` is zero, then ``x.bit_length()`` returns ``0``.

    Equivalent to::

        def bit_length(self):
            s = bin(self)       # binary representation:  bin(-37) --> '-0b100101'
            s = s.lstrip('-0b') # remove leading zeros and minus sign
            return len(s)       # len('100101') --> 6

    .. versionadded:: 2.7


Additional Methods on Float
---------------------------

The float type implements the :class:`numbers.Real` :term:`abstract base
class`. float also has the following additional methods.

.. method:: float.as_integer_ratio()

   Return a pair of integers whose ratio is exactly equal to the
   original float and with a positive denominator.  Raises
   :exc:`OverflowError` on infinities and a :exc:`ValueError` on
   NaNs.

   .. versionadded:: 2.6

.. method:: float.is_integer()

   Return ``True`` if the float instance is finite with integral
   value, and ``False`` otherwise::

      >>> (-2.0).is_integer()
      True
      >>> (3.2).is_integer()
      False

   .. versionadded:: 2.6

Two methods support conversion to
and from hexadecimal strings.  Since Python's floats are stored
internally as binary numbers, converting a float to or from a
*decimal* string usually involves a small rounding error.  In
contrast, hexadecimal strings allow exact representation and
specification of floating-point numbers.  This can be useful when
debugging, and in numerical work.


.. method:: float.hex()

   Return a representation of a floating-point number as a hexadecimal
   string.  For finite floating-point numbers, this representation
   will always include a leading ``0x`` and a trailing ``p`` and
   exponent.

   .. versionadded:: 2.6


.. method:: float.fromhex(s)

   Class method to return the float represented by a hexadecimal
   string *s*.  The string *s* may have leading and trailing
   whitespace.

   .. versionadded:: 2.6


Note that :meth:`float.hex` is an instance method, while
:meth:`float.fromhex` is a class method.

A hexadecimal string takes the form::

   [sign] ['0x'] integer ['.' fraction] ['p' exponent]

where the optional ``sign`` may by either ``+`` or ``-``, ``integer``
and ``fraction`` are strings of hexadecimal digits, and ``exponent``
is a decimal integer with an optional leading sign.  Case is not
significant, and there must be at least one hexadecimal digit in
either the integer or the fraction.  This syntax is similar to the
syntax specified in section 6.4.4.2 of the C99 standard, and also to
the syntax used in Java 1.5 onwards.  In particular, the output of
:meth:`float.hex` is usable as a hexadecimal floating-point literal in
C or Java code, and hexadecimal strings produced by C's ``%a`` format
character or Java's ``Double.toHexString`` are accepted by
:meth:`float.fromhex`.


Note that the exponent is written in decimal rather than hexadecimal,
and that it gives the power of 2 by which to multiply the coefficient.
For example, the hexadecimal string ``0x3.a7p10`` represents the
floating-point number ``(3 + 10./16 + 7./16**2) * 2.0**10``, or
``3740.0``::

   >>> float.fromhex('0x3.a7p10')
   3740.0


Applying the reverse conversion to ``3740.0`` gives a different
hexadecimal string representing the same number::

   >>> float.hex(3740.0)
   '0x1.d380000000000p+11'