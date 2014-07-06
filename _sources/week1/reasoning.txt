.. Author Sumukh Sridhara 2014

=========
Reasoning
=========

Intro
----------
		"Computers are incredibly fast, accurate and stupid; humans are incredibly slow, inaccurate and brilliant; together they are powerful beyond imagination" 

Everything that happens on a computer happens for a reason. 

Our job as programmers is to understand the reasoning so we can make the computer do what we want it do. We don't quite understanding the reasoning yet - but we can build up our understanding by working through sample problems. 

Learning to reason about code is kind of like learning how to drive. You might not understand all of the intracies of how the engine actually works - but you build an understanding of how it works by working with the inputs and observing what happens. As you use the car and use the different controls your intution about how the car works builds up. 

The Process 
---------------------
If we want to understand a piece of code, we're going to have to reason through it by following some basic steps. Every programmer follows these basic steps in some form. Over time, you'll find your own process but for now let's work through these steps. 

What does it do?
~~~~~~~~~~~~~~~~~~~~~
One of the first things you want to do when you are presented with code is to figure out what it even does.  
You want use info from the code to help you understand what the code is doing. This list is not comprehensive and you might not need to take all of the steps. 

-	Function & Variable Names
-	Domain & Range (Input & Output)
-	Conditional Statements
- 	Mentally inserting values and testing what would happen
-	Top-down line by line review of what happens

Converting to English
~~~~~~~~~~~~~~~~~~~~~

		Computers need specific instructions, Humans need simple instructions. 

With a basic understanding of the code, we can start form a hypothesis about what the code actually does. We are converting the code into English so we can better understand it.  

English:
-	This function determines whether or not a number is even.
-	This code computes a specific fibbonacci number.

Not Quite English:
-	This function takes the variable x and returns true if x modulo (%) 2 is true. 
- 	This piece of code uses iteration to sum up x and the previous value of x...

While the "Not Quite English" descriptions are accurate (and in english), they still rely on the language of programming and that makes it hard to work with. It's much easier for us to reason in a spoken language - especially since we already use English to communicate. 

Testing your hypothesis
~~~~~~~~~~~~~~~~~~~~~~~~
Now that we've formed our hypothesis, we need to verify it. There are a few ways we can approach it

-	Come up with a few valid inputs and determine what should output (using only the english translation)
-	Actually run the code with **multiple** valid inputs and observe what it returns
-	Sometimes we can't run the code, so we can act like a interpreter and mentally run through multiple inputs and verify that the result matches what we'd predict. 
- 	Mentally verify that the result that we'd predict from the code makes sense when it's used. 
- 	Bonus: Test invalid inputs. 

Exercise
~~~~~~~~~~~~~~~~~~~~

.. activecode:: doesSomething

	def doSomething(x):
		temp = 0
		while ((temp * temp) < x):
			temp += 1
		return temp*temp == x

	# Hypothesis? 

	# Test: 


Using testing first.
~~~~~~~~~~~~~~~~~~~~~
How you use the above steps depends on the type of code and your personal preferences. Sometimes it makes sense to attempt to mentally go through the steps above. Other times, it can help to work backwards - that is first attempt to test valid inputs and then form a hypothesis. This is often used when there is a lot of code because it can be faster than trying to come up with a hypothesis from just looking at the code. 


.. activecode:: test first!
	:autorun: 

	def doSomething2(x):
	    return x ** (0.5) % 1 == 0

	# Tests: 
	print(doSomething2(0))


	# Hypothesis?


English into code.
~~~~~~~~~~~~~~~~~~~~~
Now that we have an understanding of what the code does, lets start using that by turning english statements into code. 

- Idenitfy important parts of the function 
- What parts can you change to get different outputs?
- How do you read the question and identify the inputs and outputs. 


Lab
-----

Warmups.
~~~~~~~~~~~~~~~~~~~~~
3 minutes. 

What does the function doMath do? (in english, not in code) (and no math is not an acceptable answer)

.. activecode:: swimming_ex2

	def doMath(a,b,c,d):
	    if (a > b and b > c and c > d):
	        x = a * b * c * d
	        y = a + b + c + d
	    else:
	        y = 0
	        x = 0 
	    return y


Exercises
~~~~~~~~~~~~~~~


What does this do? Try to use the rules from above to form a hypothesis. 5 minutes.

.. activecode:: swimming_ex

	def summer(x, y):
	    return x + y 
	def muler(x, y):
	    return x * y
	def scuba(x, y, z):

	    return max(x, y)
	def total(x,y):
	    return scuba(summer(x,y), muler(x,y), 42)
	#Convert a call to Total to english


Side note: This is a also problem to practice your enviroment diagraming skills. You should work this out later.
Why are enviroment diagrams useful for this class and others! 

.. codelens:: swimming

	def summer(x, y):
	    return x + y 
	def muler(x, y):
	    return x * y
	def scuba(x, y, z):
	    return max(x, y)
	def total(x,y):
	    return scuba(summer(x,y), muler(x,y), 42)
	print(total(10,0)) 


*** 

What about this function that is called "doSomethingElse". What does it do in english? 

.. activecode:: doSomethingElse

	def doSomethingElse(x,y):
	    if (x - y  == 0):
	        z =  0
	    if (x - y > 0):
	        z = 1
	    if (x - y < 0):
	        z = -1

	print(doSomethingElse(6,5))


Composing
~~~~~~~~~~~


.. codelens:: composing2

	def square(x):
	    return x * x

	def next(x):
	    return x + 1

	def curry(f, g):
	    def h(x):
	        return f(g(x))
	    return h

	def foo(y):
	    x = curry(square,next)
	    return x(y)

	# What does foo(y) do ?


Fizzbuzz
~~~~~~~~~~
What does this function do (in English?)


.. activecode:: fizzbuzz

	def fizzbuzz():
	    i = 1
	    while (i <= 100):
	        msg = ""
	        if (i % 3 == 0):
	            msg += "Fizz"
	        if (i % 5 == 0):
	            msg += "Buzz" 
	        if (msg != ""):
	            print(msg)
	        else:
	            print(i)
	        i += 1

.. reveal:: reavelexer5 
    :showtitle: Show Next Fizzbuzz
    :hidetitle: Hide Exercise

    Now that we know what FizzBuzz does in English, let's make some changes. 

Assorted 
~~~~~~~~~
What does this do? 

.. activecode:: xyz

	def xyz(total, selection):
	    ways = 1
	    sel = 0
	    while sel < selection:
	        sel = sel + 1
	        ways, total = ways * total // sel, total - 1
	    return ways


