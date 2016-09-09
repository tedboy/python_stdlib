.. _typememoryview:

memoryview type
===============

.. versionadded:: 2.7

:class:`memoryview` objects allow Python code to access the internal data
of an object that supports the buffer protocol without copying.  Memory
is generally interpreted as simple bytes.

.. class:: memoryview(obj)

   Create a :class:`memoryview` that references *obj*.  *obj* must support the
   buffer protocol.  Built-in objects that support the buffer protocol include
   :class:`str` and :class:`bytearray` (but not :class:`unicode`).

   A :class:`memoryview` has the notion of an *element*, which is the
   atomic memory unit handled by the originating object *obj*.  For many
   simple types such as :class:`str` and :class:`bytearray`, an element
   is a single byte, but other third-party types may expose larger elements.

   ``len(view)`` returns the total number of elements in the memoryview,
   *view*.  The :class:`~memoryview.itemsize` attribute will give you the
   number of bytes in a single element.

   A :class:`memoryview` supports slicing to expose its data.  Taking a single
   index will return a single element as a :class:`str` object.  Full
   slicing will result in a subview::

      >>> v = memoryview('abcefg')
      >>> v[1]
      'b'
      >>> v[-1]
      'g'
      >>> v[1:4]
      <memory at 0x77ab28>
      >>> v[1:4].tobytes()
      'bce'

   If the object the memoryview is over supports changing its data, the
   memoryview supports slice assignment::

      >>> data = bytearray('abcefg')
      >>> v = memoryview(data)
      >>> v.readonly
      False
      >>> v[0] = 'z'
      >>> data
      bytearray(b'zbcefg')
      >>> v[1:4] = '123'
      >>> data
      bytearray(b'z123fg')
      >>> v[2] = 'spam'
      Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
      ValueError: cannot modify size of memoryview object

   Notice how the size of the memoryview object cannot be changed.

   :class:`memoryview` has two methods:

   .. method:: tobytes()

      Return the data in the buffer as a bytestring (an object of class
      :class:`str`). ::

         >>> m = memoryview("abc")
         >>> m.tobytes()
         'abc'

   .. method:: tolist()

      Return the data in the buffer as a list of integers. ::

         >>> memoryview("abc").tolist()
         [97, 98, 99]

   There are also several readonly attributes available:

   .. attribute:: format

      A string containing the format (in :mod:`struct` module style) for each
      element in the view.  This defaults to ``'B'``, a simple bytestring.

   .. attribute:: itemsize

      The size in bytes of each element of the memoryview.

   .. attribute:: shape

      A tuple of integers the length of :attr:`ndim` giving the shape of the
      memory as an N-dimensional array.

   .. attribute:: ndim

      An integer indicating how many dimensions of a multi-dimensional array the
      memory represents.

   .. attribute:: strides

      A tuple of integers the length of :attr:`ndim` giving the size in bytes to
      access each element for each dimension of the array.

   .. attribute:: readonly

      A bool indicating whether the memory is read only.

   .. memoryview.suboffsets isn't documented because it only seems useful for C