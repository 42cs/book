==========
Lecture 2
==========

HOF
------
.. activecode:: hof1.

   def square(x):
   		return x * x


Pairs
--------

.. codelens:: cons

    def cons(a,b):
    	def pair(message):
    		if message == 'car':
    			return a
    		else:
    			return b
    	return pair 

    z = cons(4,5)
    
    z('car')
    z('cdr')

.. codelens:: other

	def first(s):
	    return s[0]
	def rest(s):
	    return s[1]

	def getitem_link(s, i):
	    while i > 0:
	        s, i = rest(s), i - 1
	    return first(s)

	four = [1, [2, [3, [4, 'empty']]]]
	getitem_link(four, 1)