.. _bltin-file-objects:

File Objects
============

.. index::
   object: file
   builtin: file
   module: os
   module: socket

File objects are implemented using C's ``stdio`` package and can be
created with the built-in :func:`open` function.  File
objects are also returned by some other built-in functions and methods,
such as :func:`os.popen` and :func:`os.fdopen` and the :meth:`makefile`
method of socket objects. Temporary files can be created using the
:mod:`tempfile` module, and high-level file operations such as copying,
moving, and deleting files and directories can be achieved with the
:mod:`shutil` module.

When a file operation fails for an I/O-related reason, the exception
:exc:`IOError` is raised.  This includes situations where the operation is not
defined for some reason, like :meth:`seek` on a tty device or writing a file
opened for reading.

Files have the following methods:


.. method:: file.close()

   Close the file.  A closed file cannot be read or written any more. Any operation
   which requires that the file be open will raise a :exc:`ValueError` after the
   file has been closed.  Calling :meth:`close` more than once is allowed.

   As of Python 2.5, you can avoid having to call this method explicitly if you use
   the :keyword:`with` statement.  For example, the following code will
   automatically close *f* when the :keyword:`with` block is exited::

      from __future__ import with_statement # This isn't required in Python 2.6

      with open("hello.txt") as f:
          for line in f:
              print line,

   In older versions of Python, you would have needed to do this to get the same
   effect::

      f = open("hello.txt")
      try:
          for line in f:
              print line,
      finally:
          f.close()

   .. note::

      Not all "file-like" types in Python support use as a context manager for the
      :keyword:`with` statement.  If your code is intended to work with any file-like
      object, you can use the function :func:`contextlib.closing` instead of using
      the object directly.


.. method:: file.flush()

   Flush the internal buffer, like ``stdio``'s :c:func:`fflush`.  This may be a
   no-op on some file-like objects.

   .. note::

      :meth:`flush` does not necessarily write the file's data to disk.  Use
      :meth:`flush` followed by :func:`os.fsync` to ensure this behavior.


.. method:: file.fileno()

   .. index::
      pair: file; descriptor
      module: fcntl

   Return the integer "file descriptor" that is used by the underlying
   implementation to request I/O operations from the operating system.  This can be
   useful for other, lower level interfaces that use file descriptors, such as the
   :mod:`fcntl` module or :func:`os.read` and friends.

   .. note::

      File-like objects which do not have a real file descriptor should *not* provide
      this method!


.. method:: file.isatty()

   Return ``True`` if the file is connected to a tty(-like) device, else ``False``.

   .. note::

      If a file-like object is not associated with a real file, this method should
      *not* be implemented.


.. method:: file.next()

   A file object is its own iterator, for example ``iter(f)`` returns *f* (unless
   *f* is closed).  When a file is used as an iterator, typically in a
   :keyword:`for` loop (for example, ``for line in f: print line.strip()``), the
   :meth:`~file.next` method is called repeatedly.  This method returns the next input
   line, or raises :exc:`StopIteration` when EOF is hit when the file is open for
   reading (behavior is undefined when the file is open for writing).  In order to
   make a :keyword:`for` loop the most efficient way of looping over the lines of a
   file (a very common operation), the :meth:`~file.next` method uses a hidden read-ahead
   buffer.  As a consequence of using a read-ahead buffer, combining :meth:`~file.next`
   with other file methods (like :meth:`~file.readline`) does not work right.  However,
   using :meth:`seek` to reposition the file to an absolute position will flush the
   read-ahead buffer.

   .. versionadded:: 2.3


.. method:: file.read([size])

   Read at most *size* bytes from the file (less if the read hits EOF before
   obtaining *size* bytes).  If the *size* argument is negative or omitted, read
   all data until EOF is reached.  The bytes are returned as a string object.  An
   empty string is returned when EOF is encountered immediately.  (For certain
   files, like ttys, it makes sense to continue reading after an EOF is hit.)  Note
   that this method may call the underlying C function :c:func:`fread` more than
   once in an effort to acquire as close to *size* bytes as possible. Also note
   that when in non-blocking mode, less data than was requested may be
   returned, even if no *size* parameter was given.

   .. note::
      This function is simply a wrapper for the underlying
      :c:func:`fread` C function, and will behave the same in corner cases,
      such as whether the EOF value is cached.


.. method:: file.readline([size])

   Read one entire line from the file.  A trailing newline character is kept in
   the string (but may be absent when a file ends with an incomplete line). [6]_
   If the *size* argument is present and non-negative, it is a maximum byte
   count (including the trailing newline) and an incomplete line may be
   returned. When *size* is not 0, an empty string is returned *only* when EOF
   is encountered immediately.

   .. note::

      Unlike ``stdio``'s :c:func:`fgets`, the returned string contains null characters
      (``'\0'``) if they occurred in the input.


.. method:: file.readlines([sizehint])

   Read until EOF using :meth:`~file.readline` and return a list containing the lines
   thus read.  If the optional *sizehint* argument is present, instead of
   reading up to EOF, whole lines totalling approximately *sizehint* bytes
   (possibly after rounding up to an internal buffer size) are read.  Objects
   implementing a file-like interface may choose to ignore *sizehint* if it
   cannot be implemented, or cannot be implemented efficiently.


.. method:: file.xreadlines()

   This method returns the same thing as ``iter(f)``.

   .. versionadded:: 2.1

   .. deprecated:: 2.3
      Use ``for line in file`` instead.


.. method:: file.seek(offset[, whence])

   Set the file's current position, like ``stdio``'s :c:func:`fseek`. The *whence*
   argument is optional and defaults to  ``os.SEEK_SET`` or ``0`` (absolute file
   positioning); other values are ``os.SEEK_CUR`` or ``1`` (seek relative to the
   current position) and ``os.SEEK_END`` or ``2``  (seek relative to the file's
   end).  There is no return value.

   For example, ``f.seek(2, os.SEEK_CUR)`` advances the position by two and
   ``f.seek(-3, os.SEEK_END)`` sets the position to the third to last.

   Note that if the file is opened for appending
   (mode ``'a'`` or ``'a+'``), any :meth:`seek` operations will be undone at the
   next write.  If the file is only opened for writing in append mode (mode
   ``'a'``), this method is essentially a no-op, but it remains useful for files
   opened in append mode with reading enabled (mode ``'a+'``).  If the file is
   opened in text mode (without ``'b'``), only offsets returned by :meth:`tell` are
   legal.  Use of other offsets causes undefined behavior.

   Note that not all file objects are seekable.

   .. versionchanged:: 2.6
      Passing float values as offset has been deprecated.


.. method:: file.tell()

   Return the file's current position, like ``stdio``'s :c:func:`ftell`.

   .. note::

      On Windows, :meth:`tell` can return illegal values (after an :c:func:`fgets`)
      when reading files with Unix-style line-endings. Use binary mode (``'rb'``) to
      circumvent this problem.


.. method:: file.truncate([size])

   Truncate the file's size.  If the optional *size* argument is present, the file
   is truncated to (at most) that size.  The size defaults to the current position.
   The current file position is not changed.  Note that if a specified size exceeds
   the file's current size, the result is platform-dependent:  possibilities
   include that the file may remain unchanged, increase to the specified size as if
   zero-filled, or increase to the specified size with undefined new content.
   Availability:  Windows, many Unix variants.


.. method:: file.write(str)

   Write a string to the file.  There is no return value.  Due to buffering, the
   string may not actually show up in the file until the :meth:`flush` or
   :meth:`close` method is called.


.. method:: file.writelines(sequence)

   Write a sequence of strings to the file.  The sequence can be any iterable
   object producing strings, typically a list of strings. There is no return value.
   (The name is intended to match :meth:`readlines`; :meth:`writelines` does not
   add line separators.)

Files support the iterator protocol.  Each iteration returns the same result as
:meth:`~file.readline`, and iteration ends when the :meth:`~file.readline` method returns
an empty string.

File objects also offer a number of other interesting attributes. These are not
required for file-like objects, but should be implemented if they make sense for
the particular object.


.. attribute:: file.closed

   bool indicating the current state of the file object.  This is a read-only
   attribute; the :meth:`close` method changes the value. It may not be available
   on all file-like objects.


.. attribute:: file.encoding

   The encoding that this file uses. When Unicode strings are written to a file,
   they will be converted to byte strings using this encoding. In addition, when
   the file is connected to a terminal, the attribute gives the encoding that the
   terminal is likely to use (that  information might be incorrect if the user has
   misconfigured the  terminal). The attribute is read-only and may not be present
   on all file-like objects. It may also be ``None``, in which case the file uses
   the system default encoding for converting Unicode strings.

   .. versionadded:: 2.3


.. attribute:: file.errors

   The Unicode error handler used along with the encoding.

   .. versionadded:: 2.6


.. attribute:: file.mode

   The I/O mode for the file.  If the file was created using the :func:`open`
   built-in function, this will be the value of the *mode* parameter.  This is a
   read-only attribute and may not be present on all file-like objects.


.. attribute:: file.name

   If the file object was created using :func:`open`, the name of the file.
   Otherwise, some string that indicates the source of the file object, of the
   form ``<...>``.  This is a read-only attribute and may not be present on all
   file-like objects.

   .. index::
      single: universal newlines; file.newlines attribute


.. attribute:: file.newlines

   If Python was built with :term:`universal newlines` enabled (the default) this
   read-only attribute exists, and for files opened in universal newline read
   mode it keeps track of the types of newlines encountered while reading the
   file. The values it can take are ``'\r'``, ``'\n'``, ``'\r\n'``, ``None``
   (unknown, no newlines read yet) or a tuple containing all the newline types
   seen, to indicate that multiple newline conventions were encountered. For
   files not opened in universal newlines read mode the value of this attribute
   will be ``None``.


.. attribute:: file.softspace

   Boolean that indicates whether a space character needs to be printed before
   another value when using the :keyword:`print` statement. Classes that are trying
   to simulate a file object should also have a writable :attr:`softspace`
   attribute, which should be initialized to zero.  This will be automatic for most
   classes implemented in Python (care may be needed for objects that override
   attribute access); types implemented in C will have to provide a writable
   :attr:`softspace` attribute.

   .. note::

      This attribute is not used to control the :keyword:`print` statement, but to
      allow the implementation of :keyword:`print` to keep track of its internal
      state.