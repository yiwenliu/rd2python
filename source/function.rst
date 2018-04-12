Function
==========
what the function really is in python
---------------------------------------
- Besides that function is an object, even everything in Python (Yes! Even classes) are objects. 

- Various different names can be bound to the same function object.

.. code-block:: python
	:linenos:

	def first(msg):
	    print(msg)    

	first("Hello")

	second = first
	second("Hello")

- Functions can be passed as arguments to another function.

- a function can return another function, like the closure

- In fact, any object which implements the special method __call__() is termed callable. So, in the most basic sense, a decorator is a callable that returns a callable.