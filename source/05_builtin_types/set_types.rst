.. _types-set:

Set Types --- :class:`set`, :class:`frozenset`
==============================================

.. index:: object: set

A :dfn:`set` object is an unordered collection of distinct :term:`hashable` objects.
Common uses include membership testing, removing duplicates from a sequence, and
computing mathematical operations such as intersection, union, difference, and
symmetric difference.
(For other containers see the built in :class:`dict`, :class:`list`,
and :class:`tuple` classes, and the :mod:`collections` module.)


.. versionadded:: 2.4

Like other collections, sets support ``x in set``, ``len(set)``, and ``for x in
set``.  Being an unordered collection, sets do not record element position or
order of insertion.  Accordingly, sets do not support indexing, slicing, or
other sequence-like behavior.

There are currently two built-in set types, :class:`set` and :class:`frozenset`.
The :class:`set` type is mutable --- the contents can be changed using methods
like :meth:`~set.add` and :meth:`~set.remove`.  Since it is mutable, it has no
hash value and cannot be used as either a dictionary key or as an element of
another set.  The :class:`frozenset` type is immutable and :term:`hashable` ---
its contents cannot be altered after it is created; it can therefore be used as
a dictionary key or as an element of another set.

As of Python 2.7, non-empty sets (not frozensets) can be created by placing a
comma-separated list of elements within braces, for example: ``{'jack',
'sjoerd'}``, in addition to the :class:`set` constructor.

The constructors for both classes work the same:

.. class:: set([iterable])
           frozenset([iterable])

   Return a new set or frozenset object whose elements are taken from
   *iterable*.  The elements of a set must be :term:`hashable`.  To
   represent sets of sets, the inner sets must be :class:`frozenset`
   objects.  If *iterable* is not specified, a new empty set is
   returned.

   Instances of :class:`set` and :class:`frozenset` provide the following
   operations:

   .. describe:: len(s)

      Return the number of elements in set *s* (cardinality of *s*).

   .. describe:: x in s

      Test *x* for membership in *s*.

   .. describe:: x not in s

      Test *x* for non-membership in *s*.

   .. method:: isdisjoint(other)

      Return ``True`` if the set has no elements in common with *other*.  Sets are
      disjoint if and only if their intersection is the empty set.

      .. versionadded:: 2.6

   .. method:: issubset(other)
               set <= other

      Test whether every element in the set is in *other*.

   .. method:: set < other

      Test whether the set is a proper subset of *other*, that is,
      ``set <= other and set != other``.

   .. method:: issuperset(other)
               set >= other

      Test whether every element in *other* is in the set.

   .. method:: set > other

      Test whether the set is a proper superset of *other*, that is, ``set >=
      other and set != other``.

   .. method:: union(other, ...)
               set | other | ...

      Return a new set with elements from the set and all others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: intersection(other, ...)
               set & other & ...

      Return a new set with elements common to the set and all others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: difference(other, ...)
               set - other - ...

      Return a new set with elements in the set that are not in the others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: symmetric_difference(other)
               set ^ other

      Return a new set with elements in either the set or *other* but not both.

   .. method:: copy()

      Return a new set with a shallow copy of *s*.


   Note, the non-operator versions of :meth:`union`, :meth:`intersection`,
   :meth:`difference`, and :meth:`symmetric_difference`, :meth:`issubset`, and
   :meth:`issuperset` methods will accept any iterable as an argument.  In
   contrast, their operator based counterparts require their arguments to be
   sets.  This precludes error-prone constructions like ``set('abc') & 'cbs'``
   in favor of the more readable ``set('abc').intersection('cbs')``.

   Both :class:`set` and :class:`frozenset` support set to set comparisons. Two
   sets are equal if and only if every element of each set is contained in the
   other (each is a subset of the other). A set is less than another set if and
   only if the first set is a proper subset of the second set (is a subset, but
   is not equal). A set is greater than another set if and only if the first set
   is a proper superset of the second set (is a superset, but is not equal).

   Instances of :class:`set` are compared to instances of :class:`frozenset`
   based on their members.  For example, ``set('abc') == frozenset('abc')``
   returns ``True`` and so does ``set('abc') in set([frozenset('abc')])``.

   The subset and equality comparisons do not generalize to a total ordering
   function.  For example, any two non-empty disjoint sets are not equal and are not
   subsets of each other, so *all* of the following return ``False``: ``a<b``,
   ``a==b``, or ``a>b``. Accordingly, sets do not implement the :meth:`__cmp__`
   method.

   Since sets only define partial ordering (subset relationships), the output of
   the :meth:`list.sort` method is undefined for lists of sets.

   Set elements, like dictionary keys, must be :term:`hashable`.

   Binary operations that mix :class:`set` instances with :class:`frozenset`
   return the type of the first operand.  For example: ``frozenset('ab') |
   set('bc')`` returns an instance of :class:`frozenset`.

   The following table lists operations available for :class:`set` that do not
   apply to immutable instances of :class:`frozenset`:

   .. method:: update(other, ...)
               set |= other | ...

      Update the set, adding elements from all others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: intersection_update(other, ...)
               set &= other & ...

      Update the set, keeping only elements found in it and all others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: difference_update(other, ...)
               set -= other | ...

      Update the set, removing elements found in others.

      .. versionchanged:: 2.6
         Accepts multiple input iterables.

   .. method:: symmetric_difference_update(other)
               set ^= other

      Update the set, keeping only elements found in either set, but not in both.

   .. method:: add(elem)

      Add element *elem* to the set.

   .. method:: remove(elem)

      Remove element *elem* from the set.  Raises :exc:`KeyError` if *elem* is
      not contained in the set.

   .. method:: discard(elem)

      Remove element *elem* from the set if it is present.

   .. method:: pop()

      Remove and return an arbitrary element from the set.  Raises
      :exc:`KeyError` if the set is empty.

   .. method:: clear()

      Remove all elements from the set.


   Note, the non-operator versions of the :meth:`update`,
   :meth:`intersection_update`, :meth:`difference_update`, and
   :meth:`symmetric_difference_update` methods will accept any iterable as an
   argument.

   Note, the *elem* argument to the :meth:`__contains__`, :meth:`remove`, and
   :meth:`discard` methods may be a set.  To support searching for an equivalent
   frozenset, the *elem* set is temporarily mutated during the search and then
   restored.  During the search, the *elem* set should not be read or mutated
   since it does not have a meaningful value.


.. seealso::

   :ref:`comparison-to-builtin-set`
      Differences between the :mod:`sets` module and the built-in set types.