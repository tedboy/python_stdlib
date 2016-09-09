.. _typesmapping:

Mapping Types --- :class:`dict`
===============================

.. index::
   object: mapping
   object: dictionary
   triple: operations on; mapping; types
   triple: operations on; dictionary; type
   statement: del
   builtin: len

A :term:`mapping` object maps :term:`hashable` values to arbitrary objects.
Mappings are mutable objects.  There is currently only one standard mapping
type, the :dfn:`dictionary`.  (For other containers see the built in
:class:`list`, :class:`set`, and :class:`tuple` classes, and the
:mod:`collections` module.)

A dictionary's keys are *almost* arbitrary values.  Values that are not
:term:`hashable`, that is, values containing lists, dictionaries or other
mutable types (that are compared by value rather than by object identity) may
not be used as keys.  Numeric types used for keys obey the normal rules for
numeric comparison: if two numbers compare equal (such as ``1`` and ``1.0``)
then they can be used interchangeably to index the same dictionary entry.  (Note
however, that since computers store floating-point numbers as approximations it
is usually unwise to use them as dictionary keys.)

Dictionaries can be created by placing a comma-separated list of ``key: value``
pairs within braces, for example: ``{'jack': 4098, 'sjoerd': 4127}`` or ``{4098:
'jack', 4127: 'sjoerd'}``, or by the :class:`dict` constructor.

.. class:: dict(**kwarg)
           dict(mapping, **kwarg)
           dict(iterable, **kwarg)

   Return a new dictionary initialized from an optional positional argument
   and a possibly empty set of keyword arguments.

   If no positional argument is given, an empty dictionary is created.
   If a positional argument is given and it is a mapping object, a dictionary
   is created with the same key-value pairs as the mapping object.  Otherwise,
   the positional argument must be an :term:`iterable` object.  Each item in
   the iterable must itself be an iterable with exactly two objects.  The
   first object of each item becomes a key in the new dictionary, and the
   second object the corresponding value.  If a key occurs more than once, the
   last value for that key becomes the corresponding value in the new
   dictionary.

   If keyword arguments are given, the keyword arguments and their values are
   added to the dictionary created from the positional argument.  If a key
   being added is already present, the value from the keyword argument
   replaces the value from the positional argument.

   To illustrate, the following examples all return a dictionary equal to
   ``{"one": 1, "two": 2, "three": 3}``::

      >>> a = dict(one=1, two=2, three=3)
      >>> b = {'one': 1, 'two': 2, 'three': 3}
      >>> c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
      >>> d = dict([('two', 2), ('one', 1), ('three', 3)])
      >>> e = dict({'three': 3, 'one': 1, 'two': 2})
      >>> a == b == c == d == e
      True

   Providing keyword arguments as in the first example only works for keys that
   are valid Python identifiers.  Otherwise, any valid keys can be used.

   .. versionadded:: 2.2

   .. versionchanged:: 2.3
      Support for building a dictionary from keyword arguments added.


   These are the operations that dictionaries support (and therefore, custom
   mapping types should support too):

   .. describe:: len(d)

      Return the number of items in the dictionary *d*.

   .. describe:: d[key]

      Return the item of *d* with key *key*.  Raises a :exc:`KeyError` if *key*
      is not in the map.

      .. index:: __missing__()

      If a subclass of dict defines a method :meth:`__missing__` and *key*
      is not present, the ``d[key]`` operation calls that method with the key *key*
      as argument.  The ``d[key]`` operation then returns or raises whatever is
      returned or raised by the ``__missing__(key)`` call.
      No other operations or methods invoke :meth:`__missing__`. If
      :meth:`__missing__` is not defined, :exc:`KeyError` is raised.
      :meth:`__missing__` must be a method; it cannot be an instance variable::

          >>> class Counter(dict):
          ...     def __missing__(self, key):
          ...         return 0
          >>> c = Counter()
          >>> c['red']
          0
          >>> c['red'] += 1
          >>> c['red']
          1

      The example above shows part of the implementation of
      :class:`collections.Counter`.  A different ``__missing__`` method is used
      by :class:`collections.defaultdict`.

      .. versionadded:: 2.5
         Recognition of __missing__ methods of dict subclasses.

   .. describe:: d[key] = value

      Set ``d[key]`` to *value*.

   .. describe:: del d[key]

      Remove ``d[key]`` from *d*.  Raises a :exc:`KeyError` if *key* is not in the
      map.

   .. describe:: key in d

      Return ``True`` if *d* has a key *key*, else ``False``.

      .. versionadded:: 2.2

   .. describe:: key not in d

      Equivalent to ``not key in d``.

      .. versionadded:: 2.2

   .. describe:: iter(d)

      Return an iterator over the keys of the dictionary.  This is a shortcut
      for :meth:`iterkeys`.

   .. method:: clear()

      Remove all items from the dictionary.

   .. method:: copy()

      Return a shallow copy of the dictionary.

   .. method:: fromkeys(seq[, value])

      Create a new dictionary with keys from *seq* and values set to *value*.

      :func:`fromkeys` is a class method that returns a new dictionary. *value*
      defaults to ``None``.

      .. versionadded:: 2.3

   .. method:: get(key[, default])

      Return the value for *key* if *key* is in the dictionary, else *default*.
      If *default* is not given, it defaults to ``None``, so that this method
      never raises a :exc:`KeyError`.

   .. method:: has_key(key)

      Test for the presence of *key* in the dictionary.  :meth:`has_key` is
      deprecated in favor of ``key in d``.

   .. method:: items()

      Return a copy of the dictionary's list of ``(key, value)`` pairs.

      .. impl-detail::

         Keys and values are listed in an arbitrary order which is non-random,
         varies across Python implementations, and depends on the dictionary's
         history of insertions and deletions.

      If :meth:`items`, :meth:`keys`, :meth:`values`, :meth:`iteritems`,
      :meth:`iterkeys`, and :meth:`itervalues` are called with no intervening
      modifications to the dictionary, the lists will directly correspond.  This
      allows the creation of ``(value, key)`` pairs using :func:`zip`: ``pairs =
      zip(d.values(), d.keys())``.  The same relationship holds for the
      :meth:`iterkeys` and :meth:`itervalues` methods: ``pairs =
      zip(d.itervalues(), d.iterkeys())`` provides the same value for
      ``pairs``. Another way to create the same list is ``pairs = [(v, k) for
      (k, v) in d.iteritems()]``.

   .. method:: iteritems()

      Return an iterator over the dictionary's ``(key, value)`` pairs.  See the
      note for :meth:`dict.items`.

      Using :meth:`iteritems` while adding or deleting entries in the dictionary
      may raise a :exc:`RuntimeError` or fail to iterate over all entries.

      .. versionadded:: 2.2

   .. method:: iterkeys()

      Return an iterator over the dictionary's keys.  See the note for
      :meth:`dict.items`.

      Using :meth:`iterkeys` while adding or deleting entries in the dictionary
      may raise a :exc:`RuntimeError` or fail to iterate over all entries.

      .. versionadded:: 2.2

   .. method:: itervalues()

      Return an iterator over the dictionary's values.  See the note for
      :meth:`dict.items`.

      Using :meth:`itervalues` while adding or deleting entries in the
      dictionary may raise a :exc:`RuntimeError` or fail to iterate over all
      entries.

      .. versionadded:: 2.2

   .. method:: keys()

      Return a copy of the dictionary's list of keys.  See the note for
      :meth:`dict.items`.

   .. method:: pop(key[, default])

      If *key* is in the dictionary, remove it and return its value, else return
      *default*.  If *default* is not given and *key* is not in the dictionary,
      a :exc:`KeyError` is raised.

      .. versionadded:: 2.3

   .. method:: popitem()

      Remove and return an arbitrary ``(key, value)`` pair from the dictionary.

      :func:`popitem` is useful to destructively iterate over a dictionary, as
      often used in set algorithms.  If the dictionary is empty, calling
      :func:`popitem` raises a :exc:`KeyError`.

   .. method:: setdefault(key[, default])

      If *key* is in the dictionary, return its value.  If not, insert *key*
      with a value of *default* and return *default*.  *default* defaults to
      ``None``.

   .. method:: update([other])

      Update the dictionary with the key/value pairs from *other*, overwriting
      existing keys.  Return ``None``.

      :func:`update` accepts either another dictionary object or an iterable of
      key/value pairs (as tuples or other iterables of length two).  If keyword
      arguments are specified, the dictionary is then updated with those
      key/value pairs: ``d.update(red=1, blue=2)``.

      .. versionchanged:: 2.4
          Allowed the argument to be an iterable of key/value pairs and allowed
          keyword arguments.

   .. method:: values()

      Return a copy of the dictionary's list of values.  See the note for
      :meth:`dict.items`.

   .. method:: viewitems()

      Return a new view of the dictionary's items (``(key, value)`` pairs).  See
      below for documentation of view objects.

      .. versionadded:: 2.7

   .. method:: viewkeys()

      Return a new view of the dictionary's keys.  See below for documentation of
      view objects.

      .. versionadded:: 2.7

   .. method:: viewvalues()

      Return a new view of the dictionary's values.  See below for documentation of
      view objects.

      .. versionadded:: 2.7

   Dictionaries compare equal if and only if they have the same ``(key,
   value)`` pairs.


.. _dict-views:

Dictionary view objects
-----------------------

The objects returned by :meth:`dict.viewkeys`, :meth:`dict.viewvalues` and
:meth:`dict.viewitems` are *view objects*.  They provide a dynamic view on the
dictionary's entries, which means that when the dictionary changes, the view
reflects these changes.

Dictionary views can be iterated over to yield their respective data, and
support membership tests:

.. describe:: len(dictview)

   Return the number of entries in the dictionary.

.. describe:: iter(dictview)

   Return an iterator over the keys, values or items (represented as tuples of
   ``(key, value)``) in the dictionary.

   Keys and values are iterated over in an arbitrary order which is non-random,
   varies across Python implementations, and depends on the dictionary's history
   of insertions and deletions. If keys, values and items views are iterated
   over with no intervening modifications to the dictionary, the order of items
   will directly correspond.  This allows the creation of ``(value, key)`` pairs
   using :func:`zip`: ``pairs = zip(d.values(), d.keys())``.  Another way to
   create the same list is ``pairs = [(v, k) for (k, v) in d.items()]``.

   Iterating views while adding or deleting entries in the dictionary may raise
   a :exc:`RuntimeError` or fail to iterate over all entries.

.. describe:: x in dictview

   Return ``True`` if *x* is in the underlying dictionary's keys, values or
   items (in the latter case, *x* should be a ``(key, value)`` tuple).


Keys views are set-like since their entries are unique and hashable.  If all
values are hashable, so that (key, value) pairs are unique and hashable, then
the items view is also set-like.  (Values views are not treated as set-like
since the entries are generally not unique.)  Then these set operations are
available ("other" refers either to another view or a set):

.. describe:: dictview & other

   Return the intersection of the dictview and the other object as a new set.

.. describe:: dictview | other

   Return the union of the dictview and the other object as a new set.

.. describe:: dictview - other

   Return the difference between the dictview and the other object (all elements
   in *dictview* that aren't in *other*) as a new set.

.. describe:: dictview ^ other

   Return the symmetric difference (all elements either in *dictview* or
   *other*, but not in both) of the dictview and the other object as a new set.


An example of dictionary view usage::

   >>> dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}
   >>> keys = dishes.viewkeys()
   >>> values = dishes.viewvalues()

   >>> # iteration
   >>> n = 0
   >>> for val in values:
   ...     n += val
   >>> print(n)
   504

   >>> # keys and values are iterated over in the same order
   >>> list(keys)
   ['eggs', 'bacon', 'sausage', 'spam']
   >>> list(values)
   [2, 1, 1, 500]

   >>> # view objects are dynamic and reflect dict changes
   >>> del dishes['eggs']
   >>> del dishes['sausage']
   >>> list(keys)
   ['spam', 'bacon']

   >>> # set operations
   >>> keys & {'eggs', 'bacon', 'salad'}
   {'bacon'}