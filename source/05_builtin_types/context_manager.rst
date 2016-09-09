.. _typecontextmanager:

Context Manager Types
=====================

.. versionadded:: 2.5

.. index::
   single: context manager
   single: context management protocol
   single: protocol; context management

Python's :keyword:`with` statement supports the concept of a runtime context
defined by a context manager.  This is implemented using two separate methods
that allow user-defined classes to define a runtime context that is entered
before the statement body is executed and exited when the statement ends.

The :dfn:`context management protocol` consists of a pair of methods that need
to be provided for a context manager object to define a runtime context:


.. method:: contextmanager.__enter__()

   Enter the runtime context and return either this object or another object
   related to the runtime context. The value returned by this method is bound to
   the identifier in the :keyword:`as` clause of :keyword:`with` statements using
   this context manager.

   An example of a context manager that returns itself is a file object. File
   objects return themselves from __enter__() to allow :func:`open` to be used as
   the context expression in a :keyword:`with` statement.

   An example of a context manager that returns a related object is the one
   returned by :func:`decimal.localcontext`. These managers set the active
   decimal context to a copy of the original decimal context and then return the
   copy. This allows changes to be made to the current decimal context in the body
   of the :keyword:`with` statement without affecting code outside the
   :keyword:`with` statement.


.. method:: contextmanager.__exit__(exc_type, exc_val, exc_tb)

   Exit the runtime context and return a Boolean flag indicating if any exception
   that occurred should be suppressed. If an exception occurred while executing the
   body of the :keyword:`with` statement, the arguments contain the exception type,
   value and traceback information. Otherwise, all three arguments are ``None``.

   Returning a true value from this method will cause the :keyword:`with` statement
   to suppress the exception and continue execution with the statement immediately
   following the :keyword:`with` statement. Otherwise the exception continues
   propagating after this method has finished executing. Exceptions that occur
   during execution of this method will replace any exception that occurred in the
   body of the :keyword:`with` statement.

   The exception passed in should never be reraised explicitly - instead, this
   method should return a false value to indicate that the method completed
   successfully and does not want to suppress the raised exception. This allows
   context management code (such as ``contextlib.nested``) to easily detect whether
   or not an :meth:`__exit__` method has actually failed.

Python defines several context managers to support easy thread synchronisation,
prompt closure of files or other objects, and simpler manipulation of the active
decimal arithmetic context. The specific types are not treated specially beyond
their implementation of the context management protocol. See the
:mod:`contextlib` module for some examples.

Python's :term:`generator`\s and the ``contextlib.contextmanager`` :term:`decorator`
provide a convenient way to implement these protocols.  If a generator function is
decorated with the ``contextlib.contextmanager`` decorator, it will return a
context manager implementing the necessary :meth:`__enter__` and
:meth:`__exit__` methods, rather than the iterator produced by an undecorated
generator function.

Note that there is no specific slot for any of these methods in the type
structure for Python objects in the Python/C API. Extension types wanting to
define these methods must provide them as a normal Python accessible method.
Compared to the overhead of setting up the runtime context, the overhead of a
single class dictionary lookup is negligible.