Introduction
============

mock_ is a python library than can be used together with unittest_ write better
tests for your code. This document's goal is to get the reader started quickly
using mock_. None of the examples below will be difficult to follow, but a
basic knowledge of python and unittest_ library is recommended.


Mock objects
============


What is a mock object?
----------------------

A mock object is an instance of the ``Mock`` class or any of its subclasses:

.. doctest::

  >>> from mock import Mock
  >>> my_mock = Mock()

It supports attribute access and method calls:

.. doctest::

  >>> my_mock.attribute
  <Mock name='mock.attribute' id='...'>
  >>> my_mock.method()
  <Mock name='mock.method()' id='...'>

Index access is no supported by default because magic methods, those whose
names are surrounded by double undercores, require a special configuration
because of they way they are looked up internally in python. However, there is
a special subclass of ``Mock`` called ``MagicMock`` that provides a default
implementation for most magic methods including `__getitem__`:

.. doctest::

  >>> from mock import MagicMock
  >>> my_mock = MagicMock()
  >>> my_mock['index']
  <MagicMock name='mock.__getitem__()' id='...'>

Given that index access is usually required in test cases and that
``MagicMock`` provides that functionality out of the box, it's common to use
``MagicMock`` as an alias of ``Mock`` by default using ``import ... as``:

.. doctest::

  >>> from mock import MagicMock as Mock
  >>> my_mock.attribute
  <MagicMock name='mock.attribute' id='...'>
  >>> my_mock.method()
  <MagicMock name='mock.method()' id='...'>
  >>> my_mock['index']
  <MagicMock name='mock.__getitem__()' id='...'>


What makes mock objects useful for testing?
-------------------------------------------

As you may have noticed from the examples above, when an attribute is accessed
(or any other of the supported operations), another mock object is returned.
What you may have not noticed,though, is that always the same mock object is
returned, that is, on the first call a new mock object is created that is
cached and returned on any subsequent calls:

.. doctest::

  >> mock_attribute_1 = my_mock.attribute
  >> mock_attribute_2 = my_mock.attribute
  >> mock_attribute_1 is mock_attribute_2

This is a useful property because it makes possible to get the mock objects
that are going to be used in the unit under test in advance to configure them
properly depending on the need of each test case.


.. _mock: http://mock.readthedocs.org/en/latest/mock.html
.. _unittest: https://docs.python.org/2/library/unittest.html
