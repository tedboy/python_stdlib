.. _typesseq:

Sequence Types --- :class:`str`, :class:`unicode`, :class:`list`, :class:`tuple`, :class:`bytearray`, :class:`buffer`, :class:`xrange`
======================================================================================================================================

There are seven sequence types: strings, Unicode strings, lists, tuples,
bytearrays, buffers, and xrange objects.

For other containers see the built in :class:`dict` and :class:`set` classes,
and the :mod:`collections` module.


.. index::
   object: sequence
   object: string
   object: Unicode
   object: tuple
   object: list
   object: bytearray
   object: buffer
   object: xrange

String literals are written in single or double quotes: ``'xyzzy'``,
``"frobozz"``.  See :ref:`strings` for more about string literals.
Unicode strings are much like strings, but are specified in the syntax
using a preceding ``'u'`` character: ``u'abc'``, ``u"def"``. In addition
to the functionality described here, there are also string-specific
methods described in the :ref:`string-methods` section. Lists are
constructed with square brackets, separating items with commas: ``[a, b, c]``.
Tuples are constructed by the comma operator (not within square
brackets), with or without enclosing parentheses, but an empty tuple
must have the enclosing parentheses, such as ``a, b, c`` or ``()``.  A
single item tuple must have a trailing comma, such as ``(d,)``.

Bytearray objects are created with the built-in function :func:`bytearray`.

Buffer objects are not directly supported by Python syntax, but can be created
by calling the built-in function :func:`buffer`.  They don't support
concatenation or repetition.

Objects of type xrange are similar to buffers in that there is no specific syntax to
create them, but they are created using the :func:`xrange` function.  They don't
support slicing, concatenation or repetition, and using ``in``, ``not in``,
:func:`min` or :func:`max` on them is inefficient.

Most sequence types support the following operations.  The ``in`` and ``not in``
operations have the same priorities as the comparison operations.  The ``+`` and
``*`` operations have the same priority as the corresponding numeric operations.
[3]_ Additional methods are provided for :ref:`typesseq-mutable`.

This table lists the sequence operations sorted in ascending priority.
In the table, *s* and *t* are sequences of the same type; *n*, *i* and *j* are integers:

+------------------+--------------------------------+----------+
| Operation        | Result                         | Notes    |
+==================+================================+==========+
| ``x in s``       | ``True`` if an item of *s* is  | \(1)     |
|                  | equal to *x*, else ``False``   |          |
+------------------+--------------------------------+----------+
| ``x not in s``   | ``False`` if an item of *s* is | \(1)     |
|                  | equal to *x*, else ``True``    |          |
+------------------+--------------------------------+----------+
| ``s + t``        | the concatenation of *s* and   | \(6)     |
|                  | *t*                            |          |
+------------------+--------------------------------+----------+
| ``s * n, n * s`` | equivalent to adding *s* to    | \(2)     |
|                  | itself *n* times               |          |
+------------------+--------------------------------+----------+
| ``s[i]``         | *i*\ th item of *s*, origin 0  | \(3)     |
+------------------+--------------------------------+----------+
| ``s[i:j]``       | slice of *s* from *i* to *j*   | (3)(4)   |
+------------------+--------------------------------+----------+
| ``s[i:j:k]``     | slice of *s* from *i* to *j*   | (3)(5)   |
|                  | with step *k*                  |          |
+------------------+--------------------------------+----------+
| ``len(s)``       | length of *s*                  |          |
+------------------+--------------------------------+----------+
| ``min(s)``       | smallest item of *s*           |          |
+------------------+--------------------------------+----------+
| ``max(s)``       | largest item of *s*            |          |
+------------------+--------------------------------+----------+
| ``s.index(x)``   | index of the first occurrence  |          |
|                  | of *x* in *s*                  |          |
+------------------+--------------------------------+----------+
| ``s.count(x)``   | total number of occurrences of |          |
|                  | *x* in *s*                     |          |
+------------------+--------------------------------+----------+

Sequence types also support comparisons. In particular, tuples and lists
are compared lexicographically by comparing corresponding
elements. This means that to compare equal, every element must compare
equal and the two sequences must be of the same type and have the same
length. (For full details see :ref:`comparisons` in the language
reference.)

.. index::
   triple: operations on; sequence; types
   builtin: len
   builtin: min
   builtin: max
   pair: concatenation; operation
   pair: repetition; operation
   pair: subscript; operation
   pair: slice; operation
   pair: extended slice; operation
   operator: in
   operator: not in

Notes:

(1)
   When *s* is a string or Unicode string object the ``in`` and ``not in``
   operations act like a substring test.  In Python versions before 2.3, *x* had to
   be a string of length 1. In Python 2.3 and beyond, *x* may be a string of any
   length.

(2)
   Values of *n* less than ``0`` are treated as ``0`` (which yields an empty
   sequence of the same type as *s*).  Note that items in the sequence *s*
   are not copied; they are referenced multiple times.  This often haunts
   new Python programmers; consider:

      >>> lists = [[]] * 3
      >>> lists
      [[], [], []]
      >>> lists[0].append(3)
      >>> lists
      [[3], [3], [3]]

   What has happened is that ``[[]]`` is a one-element list containing an empty
   list, so all three elements of ``[[]] * 3`` are references to this single empty
   list.  Modifying any of the elements of ``lists`` modifies this single list.
   You can create a list of different lists this way:

      >>> lists = [[] for i in range(3)]
      >>> lists[0].append(3)
      >>> lists[1].append(5)
      >>> lists[2].append(7)
      >>> lists
      [[3], [5], [7]]

   Further explanation is available in the FAQ entry
   :ref:`faq-multidimensional-list`.

(3)
   If *i* or *j* is negative, the index is relative to the end of the string:
   ``len(s) + i`` or ``len(s) + j`` is substituted.  But note that ``-0`` is still
   ``0``.

(4)
   The slice of *s* from *i* to *j* is defined as the sequence of items with index
   *k* such that ``i <= k < j``.  If *i* or *j* is greater than ``len(s)``, use
   ``len(s)``.  If *i* is omitted or ``None``, use ``0``.  If *j* is omitted or
   ``None``, use ``len(s)``.  If *i* is greater than or equal to *j*, the slice is
   empty.

(5)
   The slice of *s* from *i* to *j* with step *k* is defined as the sequence of
   items with index  ``x = i + n*k`` such that ``0 <= n < (j-i)/k``.  In other words,
   the indices are ``i``, ``i+k``, ``i+2*k``, ``i+3*k`` and so on, stopping when
   *j* is reached (but never including *j*).  If *i* or *j* is greater than
   ``len(s)``, use ``len(s)``.  If *i* or *j* are omitted or ``None``, they become
   "end" values (which end depends on the sign of *k*).  Note, *k* cannot be zero.
   If *k* is ``None``, it is treated like ``1``.

(6)
   .. impl-detail::

      If *s* and *t* are both strings, some Python implementations such as
      CPython can usually perform an in-place optimization for assignments of
      the form ``s = s + t`` or ``s += t``.  When applicable, this optimization
      makes quadratic run-time much less likely.  This optimization is both
      version and implementation dependent.  For performance sensitive code, it
      is preferable to use the :meth:`str.join` method which assures consistent
      linear concatenation performance across versions and implementations.

   .. versionchanged:: 2.4
      Formerly, string concatenation never occurred in-place.


.. _string-methods:

String Methods
--------------

.. index:: pair: string; methods

Below are listed the string methods which both 8-bit strings and
Unicode objects support.  Some of them are also available on :class:`bytearray`
objects.

In addition, Python's strings support the sequence type methods
described in the :ref:`typesseq` section. To output formatted strings
use template strings or the ``%`` operator described in the
:ref:`string-formatting` section. Also, see the :mod:`re` module for
string functions based on regular expressions.

.. method:: str.capitalize()

   Return a copy of the string with its first character capitalized and the
   rest lowercased.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.center(width[, fillchar])

   Return centered in a string of length *width*. Padding is done using the
   specified *fillchar* (default is a space).

   .. versionchanged:: 2.4
      Support for the *fillchar* argument.


.. method:: str.count(sub[, start[, end]])

   Return the number of non-overlapping occurrences of substring *sub* in the
   range [*start*, *end*].  Optional arguments *start* and *end* are
   interpreted as in slice notation.


.. method:: str.decode([encoding[, errors]])

   Decodes the string using the codec registered for *encoding*. *encoding*
   defaults to the default string encoding.  *errors* may be given to set a
   different error handling scheme.  The default is ``'strict'``, meaning that
   encoding errors raise :exc:`UnicodeError`.  Other possible values are
   ``'ignore'``, ``'replace'`` and any other name registered via
   :func:`codecs.register_error`, see section :ref:`codec-base-classes`.

   .. versionadded:: 2.2

   .. versionchanged:: 2.3
      Support for other error handling schemes added.

   .. versionchanged:: 2.7
      Support for keyword arguments added.

.. method:: str.encode([encoding[,errors]])

   Return an encoded version of the string.  Default encoding is the current
   default string encoding.  *errors* may be given to set a different error
   handling scheme.  The default for *errors* is ``'strict'``, meaning that
   encoding errors raise a :exc:`UnicodeError`.  Other possible values are
   ``'ignore'``, ``'replace'``, ``'xmlcharrefreplace'``, ``'backslashreplace'`` and
   any other name registered via :func:`codecs.register_error`, see section
   :ref:`codec-base-classes`. For a list of possible encodings, see section
   :ref:`standard-encodings`.

   .. versionadded:: 2.0

   .. versionchanged:: 2.3
      Support for ``'xmlcharrefreplace'`` and ``'backslashreplace'`` and other error
      handling schemes added.

   .. versionchanged:: 2.7
      Support for keyword arguments added.

.. method:: str.endswith(suffix[, start[, end]])

   Return ``True`` if the string ends with the specified *suffix*, otherwise return
   ``False``.  *suffix* can also be a tuple of suffixes to look for.  With optional
   *start*, test beginning at that position.  With optional *end*, stop comparing
   at that position.

   .. versionchanged:: 2.5
      Accept tuples as *suffix*.


.. method:: str.expandtabs([tabsize])

   Return a copy of the string where all tab characters are replaced by one or
   more spaces, depending on the current column and the given tab size.  Tab
   positions occur every *tabsize* characters (default is 8, giving tab
   positions at columns 0, 8, 16 and so on).  To expand the string, the current
   column is set to zero and the string is examined character by character.  If
   the character is a tab (``\t``), one or more space characters are inserted
   in the result until the current column is equal to the next tab position.
   (The tab character itself is not copied.)  If the character is a newline
   (``\n``) or return (``\r``), it is copied and the current column is reset to
   zero.  Any other character is copied unchanged and the current column is
   incremented by one regardless of how the character is represented when
   printed.

      >>> '01\t012\t0123\t01234'.expandtabs()
      '01      012     0123    01234'
      >>> '01\t012\t0123\t01234'.expandtabs(4)
      '01  012 0123    01234'


.. method:: str.find(sub[, start[, end]])

   Return the lowest index in the string where substring *sub* is found within
   the slice ``s[start:end]``.  Optional arguments *start* and *end* are
   interpreted as in slice notation.  Return ``-1`` if *sub* is not found.

   .. note::

      The :meth:`~str.find` method should be used only if you need to know the
      position of *sub*.  To check if *sub* is a substring or not, use the
      :keyword:`in` operator::

         >>> 'Py' in 'Python'
         True


.. method:: str.format(*args, **kwargs)

   Perform a string formatting operation.  The string on which this method is
   called can contain literal text or replacement fields delimited by braces
   ``{}``.  Each replacement field contains either the numeric index of a
   positional argument, or the name of a keyword argument.  Returns a copy of
   the string where each replacement field is replaced with the string value of
   the corresponding argument.

      >>> "The sum of 1 + 2 is {0}".format(1+2)
      'The sum of 1 + 2 is 3'

   See :ref:`formatstrings` for a description of the various formatting options
   that can be specified in format strings.

   This method of string formatting is the new standard in Python 3, and
   should be preferred to the ``%`` formatting described in
   :ref:`string-formatting` in new code.

   .. versionadded:: 2.6


.. method:: str.index(sub[, start[, end]])

   Like :meth:`find`, but raise :exc:`ValueError` when the substring is not found.


.. method:: str.isalnum()

   Return true if all characters in the string are alphanumeric and there is at
   least one character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.isalpha()

   Return true if all characters in the string are alphabetic and there is at least
   one character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.isdigit()

   Return true if all characters in the string are digits and there is at least one
   character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.islower()

   Return true if all cased characters [4]_ in the string are lowercase and there is at
   least one cased character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.isspace()

   Return true if there are only whitespace characters in the string and there is
   at least one character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.istitle()

   Return true if the string is a titlecased string and there is at least one
   character, for example uppercase characters may only follow uncased characters
   and lowercase characters only cased ones.  Return false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.isupper()

   Return true if all cased characters [4]_ in the string are uppercase and there is at
   least one cased character, false otherwise.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.join(iterable)

   Return a string which is the concatenation of the strings in the
   :term:`iterable` *iterable*.  The separator between elements is the string
   providing this method.


.. method:: str.ljust(width[, fillchar])

   Return the string left justified in a string of length *width*. Padding is done
   using the specified *fillchar* (default is a space).  The original string is
   returned if *width* is less than or equal to ``len(s)``.

   .. versionchanged:: 2.4
      Support for the *fillchar* argument.


.. method:: str.lower()

   Return a copy of the string with all the cased characters [4]_ converted to
   lowercase.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.lstrip([chars])

   Return a copy of the string with leading characters removed.  The *chars*
   argument is a string specifying the set of characters to be removed.  If omitted
   or ``None``, the *chars* argument defaults to removing whitespace.  The *chars*
   argument is not a prefix; rather, all combinations of its values are stripped:

      >>> '   spacious   '.lstrip()
      'spacious   '
      >>> 'www.example.com'.lstrip('cmowz.')
      'example.com'

   .. versionchanged:: 2.2.2
      Support for the *chars* argument.


.. method:: str.partition(sep)

   Split the string at the first occurrence of *sep*, and return a 3-tuple
   containing the part before the separator, the separator itself, and the part
   after the separator.  If the separator is not found, return a 3-tuple containing
   the string itself, followed by two empty strings.

   .. versionadded:: 2.5


.. method:: str.replace(old, new[, count])

   Return a copy of the string with all occurrences of substring *old* replaced by
   *new*.  If the optional argument *count* is given, only the first *count*
   occurrences are replaced.


.. method:: str.rfind(sub [,start [,end]])

   Return the highest index in the string where substring *sub* is found, such
   that *sub* is contained within ``s[start:end]``.  Optional arguments *start*
   and *end* are interpreted as in slice notation.  Return ``-1`` on failure.


.. method:: str.rindex(sub[, start[, end]])

   Like :meth:`rfind` but raises :exc:`ValueError` when the substring *sub* is not
   found.


.. method:: str.rjust(width[, fillchar])

   Return the string right justified in a string of length *width*. Padding is done
   using the specified *fillchar* (default is a space). The original string is
   returned if *width* is less than or equal to ``len(s)``.

   .. versionchanged:: 2.4
      Support for the *fillchar* argument.


.. method:: str.rpartition(sep)

   Split the string at the last occurrence of *sep*, and return a 3-tuple
   containing the part before the separator, the separator itself, and the part
   after the separator.  If the separator is not found, return a 3-tuple containing
   two empty strings, followed by the string itself.

   .. versionadded:: 2.5


.. method:: str.rsplit([sep [,maxsplit]])

   Return a list of the words in the string, using *sep* as the delimiter string.
   If *maxsplit* is given, at most *maxsplit* splits are done, the *rightmost*
   ones.  If *sep* is not specified or ``None``, any whitespace string is a
   separator.  Except for splitting from the right, :meth:`rsplit` behaves like
   :meth:`split` which is described in detail below.

   .. versionadded:: 2.4


.. method:: str.rstrip([chars])

   Return a copy of the string with trailing characters removed.  The *chars*
   argument is a string specifying the set of characters to be removed.  If omitted
   or ``None``, the *chars* argument defaults to removing whitespace.  The *chars*
   argument is not a suffix; rather, all combinations of its values are stripped:

      >>> '   spacious   '.rstrip()
      '   spacious'
      >>> 'mississippi'.rstrip('ipz')
      'mississ'

   .. versionchanged:: 2.2.2
      Support for the *chars* argument.


.. method:: str.split([sep[, maxsplit]])

   Return a list of the words in the string, using *sep* as the delimiter
   string.  If *maxsplit* is given, at most *maxsplit* splits are done (thus,
   the list will have at most ``maxsplit+1`` elements).  If *maxsplit* is not
   specified or ``-1``, then there is no limit on the number of splits
   (all possible splits are made).

   If *sep* is given, consecutive delimiters are not grouped together and are
   deemed to delimit empty strings (for example, ``'1,,2'.split(',')`` returns
   ``['1', '', '2']``).  The *sep* argument may consist of multiple characters
   (for example, ``'1<>2<>3'.split('<>')`` returns ``['1', '2', '3']``).
   Splitting an empty string with a specified separator returns ``['']``.

   If *sep* is not specified or is ``None``, a different splitting algorithm is
   applied: runs of consecutive whitespace are regarded as a single separator,
   and the result will contain no empty strings at the start or end if the
   string has leading or trailing whitespace.  Consequently, splitting an empty
   string or a string consisting of just whitespace with a ``None`` separator
   returns ``[]``.

   For example, ``' 1  2   3  '.split()`` returns ``['1', '2', '3']``, and
   ``'  1  2   3  '.split(None, 1)`` returns ``['1', '2   3  ']``.


.. index::
   single: universal newlines; str.splitlines method

.. method:: str.splitlines([keepends])

   Return a list of the lines in the string, breaking at line boundaries.
   This method uses the :term:`universal newlines` approach to splitting lines.
   Line breaks are not included in the resulting list unless *keepends* is
   given and true.

   Python recognizes ``"\r"``, ``"\n"``, and ``"\r\n"`` as line boundaries for
   8-bit strings.

   For example::

      >>> 'ab c\n\nde fg\rkl\r\n'.splitlines()
      ['ab c', '', 'de fg', 'kl']
      >>> 'ab c\n\nde fg\rkl\r\n'.splitlines(True)
      ['ab c\n', '\n', 'de fg\r', 'kl\r\n']

   Unlike :meth:`~str.split` when a delimiter string *sep* is given, this
   method returns an empty list for the empty string, and a terminal line
   break does not result in an extra line::

      >>> "".splitlines()
      []
      >>> "One line\n".splitlines()
      ['One line']

   For comparison, ``split('\n')`` gives::

      >>> ''.split('\n')
      ['']
      >>> 'Two lines\n'.split('\n')
      ['Two lines', '']

.. method:: unicode.splitlines([keepends])

   Return a list of the lines in the string, like :meth:`str.splitlines`.
   However, the Unicode method splits on the following line boundaries,
   which are a superset of the :term:`universal newlines` recognized for
   8-bit strings.

   +-----------------------+-----------------------------+
   | Representation        | Description                 |
   +=======================+=============================+
   | ``\n``                | Line Feed                   |
   +-----------------------+-----------------------------+
   | ``\r``                | Carriage Return             |
   +-----------------------+-----------------------------+
   | ``\r\n``              | Carriage Return + Line Feed |
   +-----------------------+-----------------------------+
   | ``\v`` or ``\x0b``    | Line Tabulation             |
   +-----------------------+-----------------------------+
   | ``\f`` or ``\x0c``    | Form Feed                   |
   +-----------------------+-----------------------------+
   | ``\x1c``              | File Separator              |
   +-----------------------+-----------------------------+
   | ``\x1d``              | Group Separator             |
   +-----------------------+-----------------------------+
   | ``\x1e``              | Record Separator            |
   +-----------------------+-----------------------------+
   | ``\x85``              | Next Line (C1 Control Code) |
   +-----------------------+-----------------------------+
   | ``\u2028``            | Line Separator              |
   +-----------------------+-----------------------------+
   | ``\u2029``            | Paragraph Separator         |
   +-----------------------+-----------------------------+

   .. versionchanged:: 2.7

      ``\v`` and ``\f`` added to list of line boundaries.


.. method:: str.startswith(prefix[, start[, end]])

   Return ``True`` if string starts with the *prefix*, otherwise return ``False``.
   *prefix* can also be a tuple of prefixes to look for.  With optional *start*,
   test string beginning at that position.  With optional *end*, stop comparing
   string at that position.

   .. versionchanged:: 2.5
      Accept tuples as *prefix*.


.. method:: str.strip([chars])

   Return a copy of the string with the leading and trailing characters removed.
   The *chars* argument is a string specifying the set of characters to be removed.
   If omitted or ``None``, the *chars* argument defaults to removing whitespace.
   The *chars* argument is not a prefix or suffix; rather, all combinations of its
   values are stripped:

      >>> '   spacious   '.strip()
      'spacious'
      >>> 'www.example.com'.strip('cmowz.')
      'example'

   .. versionchanged:: 2.2.2
      Support for the *chars* argument.


.. method:: str.swapcase()

   Return a copy of the string with uppercase characters converted to lowercase and
   vice versa.

   For 8-bit strings, this method is locale-dependent.


.. method:: str.title()

   Return a titlecased version of the string where words start with an uppercase
   character and the remaining characters are lowercase.

   The algorithm uses a simple language-independent definition of a word as
   groups of consecutive letters.  The definition works in many contexts but
   it means that apostrophes in contractions and possessives form word
   boundaries, which may not be the desired result::

        >>> "they're bill's friends from the UK".title()
        "They'Re Bill'S Friends From The Uk"

   A workaround for apostrophes can be constructed using regular expressions::

        >>> import re
        >>> def titlecase(s):
        ...     return re.sub(r"[A-Za-z]+('[A-Za-z]+)?",
        ...                   lambda mo: mo.group(0)[0].upper() +
        ...                              mo.group(0)[1:].lower(),
        ...                   s)
        ...
        >>> titlecase("they're bill's friends.")
        "They're Bill's Friends."

   For 8-bit strings, this method is locale-dependent.


.. method:: str.translate(table[, deletechars])

   Return a copy of the string where all characters occurring in the optional
   argument *deletechars* are removed, and the remaining characters have been
   mapped through the given translation table, which must be a string of length
   256.

   You can use the :func:`~string.maketrans` helper function in the :mod:`string`
   module to create a translation table. For string objects, set the *table*
   argument to ``None`` for translations that only delete characters:

      >>> 'read this short text'.translate(None, 'aeiou')
      'rd ths shrt txt'

   .. versionadded:: 2.6
      Support for a ``None`` *table* argument.

   For Unicode objects, the :meth:`translate` method does not accept the optional
   *deletechars* argument.  Instead, it returns a copy of the *s* where all
   characters have been mapped through the given translation table which must be a
   mapping of Unicode ordinals to Unicode ordinals, Unicode strings or ``None``.
   Unmapped characters are left untouched. Characters mapped to ``None`` are
   deleted.  Note, a more flexible approach is to create a custom character mapping
   codec using the :mod:`codecs` module (see :mod:`encodings.cp1251` for an
   example).


.. method:: str.upper()

   Return a copy of the string with all the cased characters [4]_ converted to
   uppercase.  Note that ``str.upper().isupper()`` might be ``False`` if ``s``
   contains uncased characters or if the Unicode category of the resulting
   character(s) is not "Lu" (Letter, uppercase), but e.g. "Lt" (Letter, titlecase).

   For 8-bit strings, this method is locale-dependent.


.. method:: str.zfill(width)

   Return the numeric string left filled with zeros in a string of length
   *width*.  A sign prefix is handled correctly.  The original string is
   returned if *width* is less than or equal to ``len(s)``.


   .. versionadded:: 2.2.2

The following methods are present only on unicode objects:

.. method:: unicode.isnumeric()

   Return ``True`` if there are only numeric characters in S, ``False``
   otherwise. Numeric characters include digit characters, and all characters
   that have the Unicode numeric value property, e.g. U+2155,
   VULGAR FRACTION ONE FIFTH.

.. method:: unicode.isdecimal()

   Return ``True`` if there are only decimal characters in S, ``False``
   otherwise. Decimal characters include digit characters, and all characters
   that can be used to form decimal-radix numbers, e.g. U+0660,
   ARABIC-INDIC DIGIT ZERO.


.. _string-formatting:

String Formatting Operations
----------------------------

.. index::
   single: formatting, string (%)
   single: interpolation, string (%)
   single: string; formatting
   single: string; interpolation
   single: printf-style formatting
   single: sprintf-style formatting
   single: % formatting
   single: % interpolation

String and Unicode objects have one unique built-in operation: the ``%``
operator (modulo).  This is also known as the string *formatting* or
*interpolation* operator.  Given ``format % values`` (where *format* is a string
or Unicode object), ``%`` conversion specifications in *format* are replaced
with zero or more elements of *values*.  The effect is similar to the using
:c:func:`sprintf` in the C language.  If *format* is a Unicode object, or if any
of the objects being converted using the ``%s`` conversion are Unicode objects,
the result will also be a Unicode object.

If *format* requires a single argument, *values* may be a single non-tuple
object. [5]_  Otherwise, *values* must be a tuple with exactly the number of
items specified by the format string, or a single mapping object (for example, a
dictionary).

A conversion specifier contains two or more characters and has the following
components, which must occur in this order:

#. The ``'%'`` character, which marks the start of the specifier.

#. Mapping key (optional), consisting of a parenthesised sequence of characters
   (for example, ``(somename)``).

#. Conversion flags (optional), which affect the result of some conversion
   types.

#. Minimum field width (optional).  If specified as an ``'*'`` (asterisk), the
   actual width is read from the next element of the tuple in *values*, and the
   object to convert comes after the minimum field width and optional precision.

#. Precision (optional), given as a ``'.'`` (dot) followed by the precision.  If
   specified as ``'*'`` (an asterisk), the actual width is read from the next
   element of the tuple in *values*, and the value to convert comes after the
   precision.

#. Length modifier (optional).

#. Conversion type.

When the right argument is a dictionary (or other mapping type), then the
formats in the string *must* include a parenthesised mapping key into that
dictionary inserted immediately after the ``'%'`` character. The mapping key
selects the value to be formatted from the mapping.  For example:

   >>> print '%(language)s has %(number)03d quote types.' % \
   ...       {"language": "Python", "number": 2}
   Python has 002 quote types.

In this case no ``*`` specifiers may occur in a format (since they require a
sequential parameter list).

The conversion flag characters are:

+---------+---------------------------------------------------------------------+
| Flag    | Meaning                                                             |
+=========+=====================================================================+
| ``'#'`` | The value conversion will use the "alternate form" (where defined   |
|         | below).                                                             |
+---------+---------------------------------------------------------------------+
| ``'0'`` | The conversion will be zero padded for numeric values.              |
+---------+---------------------------------------------------------------------+
| ``'-'`` | The converted value is left adjusted (overrides the ``'0'``         |
|         | conversion if both are given).                                      |
+---------+---------------------------------------------------------------------+
| ``' '`` | (a space) A blank should be left before a positive number (or empty |
|         | string) produced by a signed conversion.                            |
+---------+---------------------------------------------------------------------+
| ``'+'`` | A sign character (``'+'`` or ``'-'``) will precede the conversion   |
|         | (overrides a "space" flag).                                         |
+---------+---------------------------------------------------------------------+

A length modifier (``h``, ``l``, or ``L``) may be present, but is ignored as it
is not necessary for Python -- so e.g. ``%ld`` is identical to ``%d``.

The conversion types are:

+------------+-----------------------------------------------------+-------+
| Conversion | Meaning                                             | Notes |
+============+=====================================================+=======+
| ``'d'``    | Signed integer decimal.                             |       |
+------------+-----------------------------------------------------+-------+
| ``'i'``    | Signed integer decimal.                             |       |
+------------+-----------------------------------------------------+-------+
| ``'o'``    | Signed octal value.                                 | \(1)  |
+------------+-----------------------------------------------------+-------+
| ``'u'``    | Obsolete type -- it is identical to ``'d'``.        | \(7)  |
+------------+-----------------------------------------------------+-------+
| ``'x'``    | Signed hexadecimal (lowercase).                     | \(2)  |
+------------+-----------------------------------------------------+-------+
| ``'X'``    | Signed hexadecimal (uppercase).                     | \(2)  |
+------------+-----------------------------------------------------+-------+
| ``'e'``    | Floating point exponential format (lowercase).      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'E'``    | Floating point exponential format (uppercase).      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'f'``    | Floating point decimal format.                      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'F'``    | Floating point decimal format.                      | \(3)  |
+------------+-----------------------------------------------------+-------+
| ``'g'``    | Floating point format. Uses lowercase exponential   | \(4)  |
|            | format if exponent is less than -4 or not less than |       |
|            | precision, decimal format otherwise.                |       |
+------------+-----------------------------------------------------+-------+
| ``'G'``    | Floating point format. Uses uppercase exponential   | \(4)  |
|            | format if exponent is less than -4 or not less than |       |
|            | precision, decimal format otherwise.                |       |
+------------+-----------------------------------------------------+-------+
| ``'c'``    | Single character (accepts integer or single         |       |
|            | character string).                                  |       |
+------------+-----------------------------------------------------+-------+
| ``'r'``    | String (converts any Python object using            | \(5)  |
|            | :ref:`repr() <func-repr>`).                         |       |
+------------+-----------------------------------------------------+-------+
| ``'s'``    | String (converts any Python object using            | \(6)  |
|            | :func:`str`).                                       |       |
+------------+-----------------------------------------------------+-------+
| ``'%'``    | No argument is converted, results in a ``'%'``      |       |
|            | character in the result.                            |       |
+------------+-----------------------------------------------------+-------+

Notes:

(1)
   The alternate form causes a leading zero (``'0'``) to be inserted between
   left-hand padding and the formatting of the number if the leading character
   of the result is not already a zero.

(2)
   The alternate form causes a leading ``'0x'`` or ``'0X'`` (depending on whether
   the ``'x'`` or ``'X'`` format was used) to be inserted between left-hand padding
   and the formatting of the number if the leading character of the result is not
   already a zero.

(3)
   The alternate form causes the result to always contain a decimal point, even if
   no digits follow it.

   The precision determines the number of digits after the decimal point and
   defaults to 6.

(4)
   The alternate form causes the result to always contain a decimal point, and
   trailing zeroes are not removed as they would otherwise be.

   The precision determines the number of significant digits before and after the
   decimal point and defaults to 6.

(5)
   The ``%r`` conversion was added in Python 2.0.

   The precision determines the maximal number of characters used.

(6)
   If the object or format provided is a :class:`unicode` string, the resulting
   string will also be :class:`unicode`.

   The precision determines the maximal number of characters used.

(7)
   See :pep:`237`.

Since Python strings have an explicit length, ``%s`` conversions do not assume
that ``'\0'`` is the end of the string.

.. XXX Examples?

.. versionchanged:: 2.7
   ``%f`` conversions for numbers whose absolute value is over 1e50 are no
   longer replaced by ``%g`` conversions.

.. index::
   module: string
   module: re

Additional string operations are defined in standard modules :mod:`string` and
:mod:`re`.


.. _typesseq-xrange:

XRange Type
-----------

.. index:: object: xrange

The :class:`xrange` type is an immutable sequence which is commonly used for
looping.  The advantage of the :class:`xrange` type is that an :class:`xrange`
object will always take the same amount of memory, no matter the size of the
range it represents.  There are no consistent performance advantages.

XRange objects have very little behavior: they only support indexing, iteration,
and the :func:`len` function.


.. _typesseq-mutable:

Mutable Sequence Types
----------------------

.. index::
   triple: mutable; sequence; types
   object: list

List and :class:`bytearray` objects support additional operations that allow
in-place modification of the object. Other mutable sequence types (when added
to the language) should also support these operations. Strings and tuples
are immutable sequence types: such objects cannot be modified once created.
The following operations are defined on mutable sequence types (where *x* is
an arbitrary object):

.. index::
   triple: operations on; sequence; types
   triple: operations on; list; type
   pair: subscript; assignment
   pair: slice; assignment
   pair: extended slice; assignment
   statement: del
   single: append() (list method)
   single: extend() (list method)
   single: count() (list method)
   single: index() (list method)
   single: insert() (list method)
   single: pop() (list method)
   single: remove() (list method)
   single: reverse() (list method)
   single: sort() (list method)

+------------------------------+--------------------------------+---------------------+
| Operation                    | Result                         | Notes               |
+==============================+================================+=====================+
| ``s[i] = x``                 | item *i* of *s* is replaced by |                     |
|                              | *x*                            |                     |
+------------------------------+--------------------------------+---------------------+
| ``s[i:j] = t``               | slice of *s* from *i* to *j*   |                     |
|                              | is replaced by the contents of |                     |
|                              | the iterable *t*               |                     |
+------------------------------+--------------------------------+---------------------+
| ``del s[i:j]``               | same as ``s[i:j] = []``        |                     |
+------------------------------+--------------------------------+---------------------+
| ``s[i:j:k] = t``             | the elements of ``s[i:j:k]``   | \(1)                |
|                              | are replaced by those of *t*   |                     |
+------------------------------+--------------------------------+---------------------+
| ``del s[i:j:k]``             | removes the elements of        |                     |
|                              | ``s[i:j:k]`` from the list     |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.append(x)``              | same as ``s[len(s):len(s)] =   | \(2)                |
|                              | [x]``                          |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.extend(t)`` or           | for the most part the same as  | \(3)                |
| ``s += t``                   | ``s[len(s):len(s)] = t``       |                     |
+------------------------------+--------------------------------+---------------------+
| ``s *= n``                   | updates *s* with its contents  | \(11)               |
|                              | repeated *n* times             |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.count(x)``               | return number of *i*'s for     |                     |
|                              | which ``s[i] == x``            |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.index(x[, i[, j]])``     | return smallest *k* such that  | \(4)                |
|                              | ``s[k] == x`` and ``i <= k <   |                     |
|                              | j``                            |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.insert(i, x)``           | same as ``s[i:i] = [x]``       | \(5)                |
+------------------------------+--------------------------------+---------------------+
| ``s.pop([i])``               | same as ``x = s[i]; del s[i];  | \(6)                |
|                              | return x``                     |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.remove(x)``              | same as ``del s[s.index(x)]``  | \(4)                |
+------------------------------+--------------------------------+---------------------+
| ``s.reverse()``              | reverses the items of *s* in   | \(7)                |
|                              | place                          |                     |
+------------------------------+--------------------------------+---------------------+
| ``s.sort([cmp[, key[,        | sort the items of *s* in place | (7)(8)(9)(10)       |
| reverse]]])``                |                                |                     |
+------------------------------+--------------------------------+---------------------+

Notes:

(1)
   *t* must have the same length as the slice it is  replacing.

(2)
   The C implementation of Python has historically accepted multiple parameters and
   implicitly joined them into a tuple; this no longer works in Python 2.0.  Use of
   this misfeature has been deprecated since Python 1.4.

(3)
   *t* can be any iterable object.

(4)
   Raises :exc:`ValueError` when *x* is not found in *s*. When a negative index is
   passed as the second or third parameter to the :meth:`index` method, the list
   length is added, as for slice indices.  If it is still negative, it is truncated
   to zero, as for slice indices.

   .. versionchanged:: 2.3
      Previously, :meth:`index` didn't have arguments for specifying start and stop
      positions.

(5)
   When a negative index is passed as the first parameter to the :meth:`insert`
   method, the list length is added, as for slice indices.  If it is still
   negative, it is truncated to zero, as for slice indices.

   .. versionchanged:: 2.3
      Previously, all negative indices were truncated to zero.

(6)
   The :meth:`pop` method's optional argument *i* defaults to ``-1``, so that
   by default the last item is removed and returned.

(7)
   The :meth:`sort` and :meth:`reverse` methods modify the list in place for
   economy of space when sorting or reversing a large list.  To remind you that
   they operate by side effect, they don't return the sorted or reversed list.

(8)
   The :meth:`sort` method takes optional arguments for controlling the
   comparisons.

   *cmp* specifies a custom comparison function of two arguments (list items) which
   should return a negative, zero or positive number depending on whether the first
   argument is considered smaller than, equal to, or larger than the second
   argument: ``cmp=lambda x,y: cmp(x.lower(), y.lower())``.  The default value
   is ``None``.

   *key* specifies a function of one argument that is used to extract a comparison
   key from each list element: ``key=str.lower``.  The default value is ``None``.

   *reverse* is a boolean value.  If set to ``True``, then the list elements are
   sorted as if each comparison were reversed.

   In general, the *key* and *reverse* conversion processes are much faster than
   specifying an equivalent *cmp* function.  This is because *cmp* is called
   multiple times for each list element while *key* and *reverse* touch each
   element only once.  Use :func:`functools.cmp_to_key` to convert an
   old-style *cmp* function to a *key* function.

   .. versionchanged:: 2.3
      Support for ``None`` as an equivalent to omitting *cmp* was added.

   .. versionchanged:: 2.4
      Support for *key* and *reverse* was added.

(9)
   Starting with Python 2.3, the :meth:`sort` method is guaranteed to be stable.  A
   sort is stable if it guarantees not to change the relative order of elements
   that compare equal --- this is helpful for sorting in multiple passes (for
   example, sort by department, then by salary grade).

(10)
   .. impl-detail::

      While a list is being sorted, the effect of attempting to mutate, or even
      inspect, the list is undefined.  The C implementation of Python 2.3 and
      newer makes the list appear empty for the duration, and raises
      :exc:`ValueError` if it can detect that the list has been mutated during a
      sort.

(11)
   The value *n* is an integer, or an object implementing
   :meth:`~object.__index__`.  Zero and negative values of *n* clear
   the sequence.  Items in the sequence are not copied; they are referenced
   multiple times, as explained for ``s * n`` under :ref:`typesseq`.