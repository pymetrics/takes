=====
Takes
=====


.. image:: https://img.shields.io/pypi/v/takes.svg
        :target: https://pypi.python.org/pypi/takes

.. image:: https://img.shields.io/travis/pymetrics/takes.svg
        :target: https://travis-ci.com/pymetrics/takes

.. image:: https://readthedocs.org/projects/takes/badge/?version=latest
        :target: https://takes.readthedocs.io/en/latest/?version=latest
        :alt: Documentation Status




Decorator for run-time type checks.


* Free software: MIT license
* Documentation: https://takes.readthedocs.io.


Features
--------

* Convert undefined dictionaries accepted by functions to strong data types, without changing calling code.
* Decorated functions can be called with dictionaries as before, or with instances of the desired type.

Example
=======

This function takes an undocumented dictionary. Maintainers must
read the function body to determine the expected shape of the data.

.. code :: python

    def my_function(data):
        x, y = data["x"], data["y"]
        return f"x={x}, y={y}"

    >>> my_function({"x": 1, "y": 1})
    "x=1, y=1"

Takes lets you redefine this function to accept a well-defined
data type, without needing to immediately change all of the calling
code:

.. code :: python

    from takes import takes

    @dataclass
    class Point:
        x: int
        y: int

    @takes(Point)
    def my_function(point):
        x, y = point.x, point.y
        return f"x={x}, y={y}"

    # Can still call my_function with a dictionary,
    # Takes will convert it to a point by instantiating
    # a Point using the given data as kwargs:
    >>> my_function({"x": 1, "y": 1})
    "x=1, y=1"

    # Of course, you can also call the function with the proper
    # type:
    >>> my_function(Point(x=1, y=1))
    "x=1, y=1"

Further, takes performs type checks at run-time.

Credits
-------

This package was created with Cookiecutter_ and the `pymetrics/cookiecutter-python-library`_ project template.

.. _Cookiecutter: https://github.com/audreyr/cookiecutter
.. _`pymetrics/cookiecutter-python-library`: https://github.com/pymetrics/cookiecutter-python-library
