..  Copyright (C)  Jeffrey Elkner, Peter Wentworth, Allen B. Downey, Chris
    Meyers, and Dario Mitchell.  Permission is granted to copy, distribute
    and/or modify this document under the terms of the GNU Free Documentation
    License, Version 1.3 or any later version published by the Free Software
    Foundation; with Invariant Sections being Forward, Prefaces, and
    Contributor List, no Front-Cover Texts, and no Back-Cover Texts.  A copy of
    the license is included in the section entitled "GNU Free Documentation
    License".

The Ultimate Debugging Handbook
================================

From 'How to Think Like A Computer Scientist' (with modifications) - See link at bottom for more.

Different kinds of errors can occur in a program, and it is useful to
distinguish among them in order to track them down more quickly:

#. Syntax errors are produced by Python when it is translating the source code
   into byte code. They usually indicate that there is something wrong with the
   syntax of the program. Example: Omitting the colon at the end of a ``def``
   statement yields the somewhat redundant message ``SyntaxError: invalid
   syntax``.
#. Runtime errors are produced by the runtime system if something goes wrong
   while the program is running. Most runtime error messages include
   information about where the error occurred and what functions were
   executing. Example: An infinite recursion eventually causes a runtime error
   of maximum recursion depth exceeded.
#. Semantic errors are problems with a program that compiles and runs but
   doesn't do the right thing. Example: An expression may not be evaluated in
   the order you expect, yielding an unexpected result.

The first step in debugging is to figure out which kind of error you are
dealing with. Although the following sections are organized by error type, some
techniques are applicable in more than one situation.

Syntax errors
-------------

Syntax errors are usually easy to fix once you figure out what they are.
Unfortunately, the error messages are often not helpful. The most common
messages are ``SyntaxError: invalid syntax`` and ``SyntaxError: invalid
token``, neither of which is very informative.

On the other hand, the message does tell you where in the program the problem
occurred. Actually, it tells you where Python noticed a problem, which is not
necessarily where the error is. Sometimes the error is prior to the location of
the error message, often on the preceding line.

If you are building the program incrementally, you should have a good idea
about where the error is. It will be in the last line you added.

If you are copying code from a book, start by comparing your code to the book's
code very carefully. Check every character. At the same time, remember that the
book might be wrong, so if you see something that looks like a syntax error, it
might be.

Here are some ways to avoid the most common syntax errors:

#. Make sure you are not using a Python keyword for a variable name.
#. Check that you have a colon at the end of the header of every compound
   statement, including ``for``, ``while``, ``if``, and ``def`` statements.
#. Check that indentation is consistent. You may indent with either spaces or
   tabs but it's best not to mix them. Each level should be nested the same
   amount.
#. Make sure that any strings in the code have matching quotation marks.
#. If you have multiline strings with triple quotes (single or double), make
   sure you have terminated the string properly. An unterminated string may
   cause an ``invalid token`` error at the end of your program, or it may treat
   the following part of the program as a string until it comes to the next
   string. In the second case, it might not produce an error message at all!
#. An unclosed bracket -- (, {, or [ -- makes Python continue with the next
   line as part of the current statement. Generally, an error occurs almost
   immediately in the next line.
#. Check for the classic ``=`` instead of ``==`` inside a conditional.

If nothing works, move on to the next section...


I can't get my program to run no matter what I do.
--------------------------------------------------

If the compiler says there is an error and you don't see it, that might be
because you and the compiler are not looking at the same code. Check your
programming environment to make sure that the program you are editing is the
one Python is trying to run. If you are not sure, try putting an obvious and
deliberate syntax error at the beginning of the program. Now run (or import) it
again. If the compiler doesn't find the new error, there is probably something
wrong with the way your environment is set up.

If this happens, one approach is to start again with a new program like Hello,
World!, and make sure you can get a known program to run.  Then gradually add
the pieces of the new program to the working one.


Runtime errors
--------------

Once your program is syntactically correct, Python can import it and at least
start running it. What could possibly go wrong?



My program does absolutely nothing.
-----------------------------------

This problem is most common when your file consists of functions and classes
but does not actually invoke anything to start execution. This may be
intentional if you only plan to import this module to supply classes and
functions.

If it is not intentional, make sure that you are invoking a function to start
execution, or execute one from the interactive prompt. Also see the Flow of
Execution section below.


My program hangs.
-----------------

If a program stops and seems to be doing nothing, we say it is hanging. Often
that means that it is caught in an infinite loop or an infinite recursion.

#. If there is a particular loop that you suspect is the problem, add a
   ``print`` statement immediately before the loop that says entering the loop
   and another immediately after that says exiting the loop.
#. Run the program. If you get the first message and not the second, you've got
   an infinite loop. Go to the Infinite Loop section below.
#. Most of the time, an infinite recursion will cause the program to run for a
   while and then produce a RuntimeError: Maximum recursion depth exceeded
   error. If that happens, go to the Infinite Recursion
   section below.
#. If you are not getting this error but you suspect there is a problem with a
   recursive method or function, you can still use the techniques in the
   Infinite Recursion section.
#. If neither of those steps works, start testing other loops and other
   recursive functions and methods.
#. If that doesn't work, then it is possible that you don't understand the flow
   of execution in your program. Go to the Flow of Execution section below.


Infinite Loop
-------------

If you think you have an infinite loop and you think you know what loop is
causing the problem, add a ``print`` statement at the end of the loop that
prints the values of the variables in the condition and the value of the
condition.

For example:

.. sourcecode:: python
    
    while x > 0 and y < 0:
        # do something to x
        # do something to y
       
        print("x: ", x)
        print("y: ", y)
        print("condition: ", (x > 0 and y < 0))

Now when you run the program, you will see three lines of output for each time
through the loop. The last time through the loop, the condition should be
``false``. If the loop keeps going, you will be able to see the values of ``x``
and ``y``, and you might figure out why they are not being updated correctly.

In a development environment like PyScripter, one can also set a breakpoint
at the start of the loop, and single-step through the loop.  While you do
this, inspect the values of ``x`` and ``y`` by hovering your cursor over 
them. 

Of course, all programming and debugging require that you have a good mental 
model of what the algorithm ought to be doing: if you don't understand what 
ought to happen to ``x`` and ``y``, printing or inspecting its value is
of little use. Probably the best place to debug the code is away from 
your computer, working on your understanding of what should be happening. 


Infinite Recursion
------------------

Most of the time, an infinite recursion will cause the program to run for a
while and then produce a ``Maximum recursion depth exceeded`` error.

If you suspect that a function or method is causing an infinite recursion,
start by checking to make sure that there is a base case.  In other words,
there should be some condition that will cause the function or method to return
without making a recursive invocation. If not, then you need to rethink the
algorithm and identify a base case.

If there is a base case but the program doesn't seem to be reaching it, add a
``print`` statement at the beginning of the function or method that prints the
parameters. Now when you run the program, you will see a few lines of output
every time the function or method is invoked, and you will see the parameters.
If the parameters are not moving toward the base case, you will get some ideas
about why not.

Once again, if you have an environment that supports easy single-stepping,
breakpoints, and inspection, learn to use them well. It is our opinion that
walking through code step-by-step builds the best and most accurate mental
model of how computation happens. Use it if you have it!


Flow of Execution
-----------------

If you are not sure how the flow of execution is moving through your program,
add ``print`` statements to the beginning of each function with a message like
entering function ``foo``, where ``foo`` is the name of the function.

Now when you run the program, it will print a trace of each function as it is
invoked.

If you're not sure, step through the program with your debugger.

When I run the program I get an exception.
------------------------------------------

If something goes wrong during runtime, Python prints a message that includes
the name of the exception, the line of the program where the problem occurred,
and a traceback.

Put a breakpoint on the line causing the exception, and look around!

The traceback identifies the function that is currently running, and then the
function that invoked it, and then the function that invoked *that*, and so on.
In other words, it traces the path of function invocations that got you to
where you are. It also includes the line number in your file where each of
these calls occurs.

The first step is to examine the place in the program where the error occurred
and see if you can figure out what happened. These are some of the most common
runtime errors:

NameError
    You are trying to use a variable that doesn't exist in the current
    environment. Remember that local variables are local. You cannot refer to
    them from outside the function where they are defined.

TypeError
    There are several possible causes:

    #. You are trying to use a value improperly. Example: indexing a
       string, list, or tuple with something other than an integer.
    #. There is a mismatch between the items in a format string and the
       items passed for conversion. This can happen if either the number of
       items does not match or an invalid conversion is called for.
    #. You are passing the wrong number of arguments to a function or
       method. For methods, look at the method definition and check that the
       first parameter is ``self``. Then look at the method invocation; make
       sure you are invoking the method on an object with the right type and
       providing the other arguments correctly.

KeyError
    You are trying to access an element of a dictionary using a key value that
    the dictionary does not contain.

AttributeError
    You are trying to access an attribute or method that does not exist.

IndexError
    The index you are using to access a list, string, or tuple is greater than
    its length minus one. Immediately before the site of the error, add a
    ``print`` statement to display the value of the index and the length of the
    array. Is the array the right size? Is the index the right value?



I added so many ``print`` statements I get inundated with output.
-----------------------------------------------------------------

One of the problems with using ``print`` statements for debugging is
that you can end up buried in output. There are two ways to proceed:
simplify the output or simplify the program.

To simplify the output, you can remove or comment out ``print``
statements that aren't helping, or combine them, or format the output
so it is easier to understand.

To simplify the program, there are several things you can do. First,
scale down the problem the program is working on. For example, if you
are sorting an array, sort a *small* array. If the program takes input
from the user, give it the simplest input that causes the problem.

Second, clean up the program. Remove dead code and reorganize the
program to make it as easy to read as possible. For example, if you
suspect that the problem is in a deeply nested part of the program,
try rewriting that part with simpler structure. If you suspect a large
function, try splitting it into smaller functions and testing them
separately.

Often the process of finding the minimal test case leads you to the
bug. If you find that a program works in one situation but not in
another, that gives you a clue about what is going on.

Similarly, rewriting a piece of code can help you find subtle bugs. If
you make a change that you think doesn't affect the program, and it
does, that can tip you off.

You can also wrap your debugging print statements in some
condition, so that you suppress much of the output. For example, if
you are trying to find an element using a binary search, and it is
not working, you might code up a debugging print statement inside
a conditional:  if the range of candidate elements is less that 6,
then print debugging information, otherwise don't print. 

Similarly, breakpoints can be made conditional: you can set a breakpoint
on a statement, then edit the breakpoint to say "only break if this
expression becomes true".

Semantic errors
---------------

In some ways, semantic errors are the hardest to debug, because the
compiler and the runtime system provide no information about what is
wrong. Only you know what the program is supposed to do, and only you
know that it isn't doing it.

The first step is to make a connection between the program text and
the behavior you are seeing. You need a hypothesis about what the
program is actually doing. One of the things that makes that hard is
that computers run so fast.

You will often wish that you could slow the program down to human
speed, and with some debuggers you can. But the time it takes to
insert a few well-placed ``print`` statements is often short compared to
setting up the debugger, inserting and removing breakpoints, and
walking the program to where the error is occurring.


My program doesn't work.
------------------------

You should ask yourself these questions:


#. Is there something the program was supposed to do but which doesn't
   seem to be happening? Find the section of the code that performs that
   function and make sure it is executing when you think it should.
#. Is something happening that shouldn't? Find code in your program
   that performs that function and see if it is executing when it
   shouldn't.
#. Is a section of code producing an effect that is not what you
   expected? Make sure that you understand the code in question,
   especially if it involves invocations to functions or methods in other
   Python modules. Read the documentation for the functions you invoke.
   Try them out by writing simple test cases and checking the results.


In order to program, you need to have a mental model of how programs
work. If you write a program that doesn't do what you expect, very
often the problem is not in the program; it's in your mental model.

The best way to correct your mental model is to break the program into
its components (usually the functions and methods) and test each
component independently. Once you find the discrepancy between your
model and reality, you can solve the problem.

Of course, you should be building and testing components as you
develop the program. If you encounter a problem, there should be only
a small amount of new code that is not known to be correct.


I've got a big hairy expression and it doesn't do what I expect.
----------------------------------------------------------------

Writing complex expressions is fine as long as they are readable, but
they can be hard to debug. It is often a good idea to break a complex
expression into a series of assignments to temporary variables.

For example:

.. sourcecode:: python
    
    self.hands[i].addCard (self.hands[self.findNeighbor(i)].popCard())

This can be rewritten as:

.. sourcecode:: python

    
    neighbor = self.findNeighbor (i)
    pickedCard = self.hands[neighbor].popCard()
    self.hands[i].addCard (pickedCard)

The explicit version is easier to read because the variable names provide
additional documentation, and it is easier to debug because you can check the
types of the intermediate variables and display or inspect their values.

Another problem that can occur with big expressions is that the order of
evaluation may not be what you expect. For example, if you are translating the
expression ``x/2pi`` into Python, you might write:

.. sourcecode:: python
    
    y = x / 2 * math.pi;

That is not correct because multiplication and division have the same
precedence and are evaluated from left to right. So this expression computes
``(x/2)pi``.

A good way to debug expressions is to add parentheses to make the order of
evaluation explicit:

.. sourcecode:: python
    
    y = x / (2 * math.pi);

Whenever you are not sure of the order of evaluation, use parentheses.  Not
only will the program be correct (in the sense of doing what you intended), it
will also be more readable for other people who haven't memorized the rules of
precedence.


I've got a function or method that doesn't return what I expect.
----------------------------------------------------------------

If you have a ``return`` statement with a complex expression, you don't have a
chance to print the ``return`` value before returning. Again, you can use a
temporary variable. For example, instead of:

.. sourcecode:: python
    
    return self.hands[i].removeMatches()

you could write:

.. sourcecode:: python
    
    count = self.hands[i].removeMatches()
    return count

Now you have the opportunity to display or inspect the value of ``count`` before
returning.


I'm really, really stuck and I need help.
-----------------------------------------

First, try getting away from the computer for a few minutes. Computers emit
waves that affect the brain, causing these effects:

#. Frustration and/or rage.
#. Superstitious beliefs ( the computer hates me ) and magical thinking ( the
   program only works when I wear my hat backward ).
#. Random-walk programming (the attempt to program by writing every possible
   program and choosing the one that does the right thing).

If you find yourself suffering from any of these symptoms, get up and go for a
walk. When you are calm, think about the program. What is it doing? What are
some possible causes of that behavior? When was the last time you had a working
program, and what did you do next?

Sometimes it just takes time to find a bug. We often find bugs when we are away
from the computer and let our minds wander. Some of the best places to find
bugs are trains, showers, and in bed, just before you fall asleep.


No, I really need help.
-----------------------

It happens. Even the best programmers occasionally get stuck.  Sometimes you
work on a program so long that you can't see the error.  A fresh pair of eyes
is just the thing.

Before you bring someone else in, make sure you have exhausted the techniques
described here. Your program should be as simple as possible, and you should be
working on the smallest input that causes the error. You should have ``print``
statements in the appropriate places (and the output they produce should be
comprehensible). You should understand the problem well enough to describe it
concisely.

When you bring someone in to help, be sure to give them the information they
need:

#. If there is an error message, what is it and what part of the program does
   it indicate?
#. What was the last thing you did before this error occurred? What were the
   last lines of code that you wrote, or what is the new test case that fails?
#. What have you tried so far, and what have you learned?

Good instructors and helpers will also do something that should not 
offend you: they won't believe when you tell them *"I'm sure all the input
routines are working just fine, and that I've set up the data correctly!"*.
They will want to validate and check things for themselves.  
After all, your program has a bug.  
Your understanding and inspection of the code have not found it yet. So you
should expect to have your assumptions challenged.  And as you gain skills
and help others, you'll need to do the same for them.

When you find the bug, take a second to think about what you could have done to
find it faster. Next time you see something similar, you will be able to find
the bug more quickly.

Remember, the goal is not just to make the program work. The goal is to learn
how to make the program work.

How to Avoid Debugging
-----------------------

Perhaps the most important lesson in debugging is that it is **largely avoidable** -- if you work carefully.

1.  **Start Small**  This is probably the single biggest piece of advice for programmers at every level.  Of course its tempting to sit down and crank out an entire program at once.  But, when the program -- inevitably -- does not work then you have a myriad of options for things that might be wrong.  Where to start?  Where to look first?  How to figure out what went wrong?  I'll get to that in the next section.  So, start with something really small.  Maybe just two lines and then make sure that runs ok.  Hitting the run button is quick and easy, and gives you immediate feedback about whether what you have just done is ok or not.  Another immediate benefit of having something small working is that you have something to turn in.  Turning in a small, incomplete program, is almost always better than nothing.


2.  **Keep it working**  Once you have a small part of your program working the next step is to figure out something small to add to it.  If you keep adding small pieces of the program one at a time, it is much easier to figure out what went wrong, as it is most likely that the problem is going to be in the new code you have just added.  Less new code means its easier to figure out where the problem is.

This notion of **Get something working and keep it working** is a mantra that you can repeat throughout your career as a programmer.  It's a great way to avoid the frustrations mentioned above.  Think of it this way.  Every time you have a little success, your brain releases a tiny bit of chemical that makes you happy.  So, you can keep yourself happy and make programming more enjoyable by creating lots of small victories for yourself.


Lab
~~~~~ 

Ok, let's look at an example.  Let's solve the problem posed in question 3 at the end of the Simple Python Data chapter.  Ask the user for the time now (in hours 0 - 23), and ask for the number of hours to wait. Your program should output what the time will be on the clock when the alarm goes off.

So, where to start?  The problem requires two pieces of input from the user, so let's start there and make sure we can get the data we need.

.. activecode:: db_ex3_1

   current_time = input("what is the current time (in hours)?")
   wait_time = input("How many hours do you want to wait")

   print(current_time)
   print(wait_time)


So far so good.  Now let's take the next step.  We need to figure out what the time will be after waiting ``wait_time`` number of hours.  A good first approximation to that is to simply add ``wait_time`` to ``current_time`` and print out the result.  So lets try that.

.. activecode:: db_ex3_2

   current_time = input("What is the current time (in hours 0 - 23)?")
   wait_time = input("How many hours do you want to wait")

   print(current_time)
   print(wait_time)

   final_time = current_time + wait_time
   print(final_time)

Hmm, when you run that example you see that something funny has happened.

.. mchoicemf:: db_q_ex3_1
   :answer_a: Python is stupid and does not know how to add properly.
   :answer_b: There is nothing wrong here.
   :answer_c: Python is doing string concatenation, not integer addition.
   :correct: c
   :feedback_a: No, Python is probabaly not broken.
   :feedback_b: No, try adding the two numbers together yourself, you will definitely get a different result.
   :feedback_c: Yes!  Remember that input returns a string.  Now we will need to convert the string to an integer

   Which of the following best describes what is wrong with the  previous example?

This error was probably pretty simple to spot, because we printed out the value of ``final_time`` and it is easy to see that the numbers were just concatenated together rather than added.  So what do we do about the problem?  We will need to convert both ``current_time`` and ``wait_time`` to ``int``.  At this stage of your programming development, it can be a good idea to include the type of the variable in the variable name itself.  So let's look at another iteration of the program that does that, and the conversion to integer.


.. activecode:: db_ex3_3

   current_time_str = input("What is the current time (in hours 0-23)?")
   wait_time_str = input("How many hours do you want to wait")

   current_time_int = int(current_time_str)
   wait_time_int = int(wait_time_str)

   final_time_int = current_time_int + wait_time_int
   print(final_time_int)


.. index:: boundary conditions, testing, debugging

Now, thats a lot better, and in fact depending on the hours you chose, it may be exactly right.  If you entered 8 for the current time and 5 for the wait time then 13 is correct.  But if you entered 17 (5pm) for the hours and 9 for the wait time then the result of 26 is not correct.  This illustrates an important aspect of **testing**, which is that it is important to test your code on a range of inputs.  It is especially important to test your code on **boundary conditions**.  In this case you would want to test your program for hours including 0, 23, and some in between.  You would want to test your wait times for 0, and some really large numbers.  What about negative numbers?  Negative numbers don't make sense, but since we don't really have the tools to deal with telling the user when something is wrong we will not worry about that just yet.  

So finally we need to account for those numbers that are bigger than 23.  For this we will need one final step, using the modulo operator.

.. activecode:: db_ex3_4

   current_time_str = input("What is the current time (in hours 0-23)?")
   wait_time_str = input("How many hours do you want to wait")

   current_time_int = int(current_time_str)
   wait_time_int = int(wait_time_str)

   final_time_int = current_time_int + wait_time_int
   
   final_answer = final_time_int % 24

   print("The time after waiting is: ", final_answer)

Of course even in this simple progression, there are other ways you could have gone astray.  We'll look at some of those and how you track them down in the next section.


----


The ultimate guide to error messages
-------------------------

Many problems in your program will lead to an error message.  For example as I was writing and testing this chapter of the book I wrote the following version of the example program in the previous section.

.. sourcecode:: python

   current_time_str = input("What is the current time (in hours 0-23)?")
   wait_time_str = input("How many hours do you want to wait")

   current_time_int = int(current_time_str)
   wait_time_int = int(wait_time_int)

   final_time_int = current_time_int + wait_time_int
   print(final_time_int)

Can you see what is wrong, just by looking at the code?  Maybe, maybe not.  Our brain tends to see what we think is there, so sometimes it is very hard to find the problem just by looking at the code.  Especially when it is our own code and we are sure that we have done everything right!

Let's try the program again, but this time in an activecode:

.. activecode:: db_ex3_5

   current_time_str = input("What is the current time (in hours 0-23)?")
   wait_time_str = input("How many hours do you want to wait")

   current_time_int = int(current_time_str)
   wait_time_int = int(wait_time_int)

   final_time_int = current_time_int + wait_time_int
   print(final_time_int)


Aha!  Now we have an error message that might be useful.  The name error tells us that  ``wait_time_int`` is not defined.  It also tells us that the error is on line 5.  Thats **really** useful information.  Now look at line five and you will see that ``wait_time_int`` is used on both the left and the right hand side of the assignment statement. 

.. mchoicemf:: db_qex32
   :answer_a: You cannot use a variable on both the left and right hand sides of an assignment statement.
   :answer_b: wait_time_int does not have a value so it cannot be used on the right hand side.
   :answer_c: This is not really an error, Python is broken.
   :correct: b
   :feedback_a: No, You can, as long as all the variables on the right hand side already have values.
   :feedback_b: Yes.  Variables must already have values in order to be used on the right hand side.
   :feedback_c: No, No, No!

   Which of the following explains why ``wait_time_int = int(wait_time_int)`` is an error.

Nearly 90% of the error messages encountered for this  problem are ParseError, TypeError, NameError, or ValueError.  We will look at these errors in three stages:

* First we will define what these four error messages mean.
* Then, we will look at some examples that cause these errors to occur.
* Finally we will look at ways to help uncover the root cause of these messages.


ParseError
^^^^^^^^^^

Parse errors happen when you make an error in the syntax of your program.  Syntax errors are like making grammatical errors in writing.  If you don't use periods and commas in your writing then you are making it hard for other readers to figure out what you are trying to say.  Similarly Python has certain grammatical rules that must be followed or else Python can't figure out what you are trying to say.

Usually ParseErrors can be traced back to missing punctuation characters, such as parenthesis, qutation marks, or commas. Remember that in Python commas are used to separate parameters to functions.  Paretheses must be balanced, or else Python thinks that you are trying to include everything that follows as a parameter to some function.

Here are a couple examples of Parse errors in the example program we have been using.  See if you can figure out what caused them.

.. tabbed:: db_tabs1

    .. tab:: Question

        Find and fix the error in the following code.

        .. activecode:: db_ex3_6

           current_time_str = input("What is the current time (in hours 0-23)?")
           wait_time_str = input("How many hours do you want to wait"

           current_time_int = int(current_time_str)
           wait_time_int = int(wait_time_str)

           final_time_int = current_time_int + wait_time_int
           print(final_time_int)

    .. tab:: Answer

        .. sourcecode:: python

           current_time_str = input("What is the current time (in hours 0-23)?")
           wait_time_str = input("How many hours do you want to wait"

           current_time_int = int(current_time_str)
           wait_time_int = int(wait_time_str)

           final_time_int = current_time_int + wait_time_int
           print(final_time_int)

        Since the error message points us to line 4 this might be a bit confusing.  If you look at line four carefully you will see that there is no problem with the syntax.  So, in this case the next step should be to back up and look at the previous line.  In this case if you look at line 2 carefully you will see that there is a missing right parenthesis at the end of the line.  Remember that parenthses must be balanced.  Since Python allows statements to continue over multiple lines inside parentheses python will continue to scan subsequent lines looking for the balancing right parenthesis.  However in this case it finds the name ``current_time_int`` and it will want to interpret that as another parameter to the input function.  But, there is not a comma to separate the previous string from the variable so as far as Python is concerned the error here is a missing comma.  From your perspective its a missing parenthesis.

**Finding Clues**  How can you help yourself find these problems?  One trick that can be very valuable in this situation is to simply start by commenting out the line number that is flagged as having the error.  If you comment out line four, the error message now changes to point to line 5.  Now you ask yourself, am I really that bad that I have two lines in a row that have errors on them?  Maybe, so taken to the extreme, you could comment out all of the remaining lines in the program. Now the error message changes to ``TokenError: EOF in multi-line statement``  This is a very technical way of saying that Python got to the end of file (EOF) while it was still looking for something.  In this case a right parenthesis.



.. tabbed:: db_tabs2

    .. tab:: Question

        Find and fix the error in the following code.

        .. activecode:: db_ex3_7

           current_time_str = input("What is the "current time" (in hours 0-23)?")
           wait_time_str = input("How many hours do you want to wait")

           current_time_int = int(current_time_str)
           wait_time_int = int(wait_time_str)

           final_time_int = current_time_int + wait_time_int
           print(final_time_int)

    .. tab:: Answer

        .. sourcecode:: python

           current_time_str = input("What is the "current time" (in hours 0-23)?")
           wait_time_str = input("How many hours do you want to wait")

           current_time_int = int(current_time_str)
           wait_time_int = int(wait_time_str)

           final_time_int = current_time_int + wait_time_int
           print(final_time_int)

        The error message points you to line 1 and in this case that is exactly where the error occurs. In this case your biggest clue is to notice the difference in  highlighting on the line.  Notice that the words "current time" are a different color than those around them.  Why is this?  Because "current time" is in double quotes inside another pair of double quotes Python things that you are finishing off one string, then you have some other names and findally another string.  But you haven't separated these names or strings by commas, and you haven't added them together with the concatenation operator (+).  So, there are several corrections you could make.  First you could make the argument to input be as follows:  ``"What is the 'current time' (in hours 0-23)"``  Notice that here we have correctly used single quotes inside double quotes.   Another option is to simply remove the extra double quotes.  Why were you quoting "current time" anyway?  ``"What is the current time (in hours 0-23)"``

**Finding Clues**  If you follow the same advice as for the last problem, comment out line one, you will immediately get a different error message.  Here's where you need to be very careful and not panic.  The error message you get now is: ``NameError: name 'current_time_str' is not defined on line 4``.  You might be very tempted to think that this is somehow related to the earlier problem and immediately conclude that there is something wrong with the variable name ``current_time_str`` but if you reflect for a minute  You will see that by commenting out line one you have caused a new and unrelated error.  That is you have commented out the creation of the name ``current_time_str``.  So of course when you want to convert it to an ``int`` you will get the NameError.  Yes, this can be confusing, but it will become much easier with experience.  It's also important to keep calm, and evaluate each new clue carefully so you don't waste time chasing problems that are not really there.  

Uncomment line 1 and you are back to the ParseError.  Another track is to eliminate a possible source of error.  Rather than commenting out the entire line you might just try to assign ``current_time_str`` to a constant value.  For example you might make line one look like this:  ``current_time_str = "10"  #input("What is the "current time" (in hours 0-23)?")``.  Now you have assigned ``current_time_str`` to the string 10, and commented out the input statement.  And now the program works!  So you conclude that the problem must have something to do with the input function.


TypeError
^^^^^^^^^

TypeErrors occur when you you try to combine two objects that are not compatible.  For example you try to add together an integer and a string.  Usually type errors can be isolated to lines that are using mathematical operators, and usually the line number given by the error message is an accurate indication of the line.

Here's an example of a type error created by a Polish learner.  See if you can find and fix the error.

.. activecode:: db_ex3_8

    a = input('wpisuj cieciu godzine')
    x = input('wpisuj ile godzin cieciu')
    int(x)
    int(a)
    h = x // 24
    s = x % 24
    print (h, s)
    a = a + s
    print ('godzina teraz %s' %a) 



.. reveal:: dbex38_rev
    :showtitle: Show me the Solution
    :hidetitle: Hide

    .. admonition:: Solution

        In finding this error there are few lessons to think about.  First, you may find it very disconcerting that you cannot understand the whole program.  Unless you speak Polish then this won't be an issue.  But, learning what you can ignore, and what you need to focus on is a very important part of the debugging process.  Second, types and good variable names are important and can be very helpful.  In this case a and x are not particularly helpful names, and in particular they do not help you think about the types of your variables, which as the error message implies is the root of the problem here.  The rest of the lessons we will get back to in a minute.

        The error message provided to you gives you a pretty big hint.  ``TypeError: unsupported operand type(s) for FloorDiv: 'str' and 'number' on line: 5``  On line five we are trying to use integer division on x and 24.  The error message tells you that you are tyring to divide a string by a number.  In this case you know that 24 is a number so x must be a string.  But how?  You can see the function call on line 3 where you are converting x to an integer.  ``int(x)`` or so you think.  This is lesson three and is one of the most common errors we see in introductory programming.  What is the difference between ``int(x)`` and ``x = int(x)``

        * The expression ``int(x)`` converts the string referenced by x to an integer but it does not store it anywhere.  It is very common to assume that ``int(x)`` somehow changes x itself, as that is what you are intending!  The thing that makes this very tricky is that ``int(x)`` is a valid expression, so it doesn't cause any kind of error, but rather the error happens later on in the program.

        * The assignment statement  ``x = int(x)`` is very different.  Again, the ``int(x)`` expression converts the string referenced by x to an integer, but this time it also changes what x references so that x now refers to the integer value returned by the ``int`` function.  

        So, the solution to this problem is to change lines 3 and 4 so they are assignment statements.


**Finding Clues**  One thing that can help you in this situation is to print out the values and the types of the variables involved in the statement that is causing the error.  You might try adding a print statement after line 4 ``print(x, type(x))``  You will see that at least we have confirmed that x is of type string.  Now you need to start to work backward through the program.  You need to ask yourself, where is x used in the program?  x is used on lines 2, 3, and of course 5 and 6 (where we are getting an error).  So maybe you move the print statement to be after line 2 and again after 3.  Line Three is where you expect the value of x to be changed to an integer.  Could line 4 be mysteriously changine x back to a string?  Not very likely.  So the value and type of x is just what you would expect it to be after line 2, but not after line 3.  This helps you isolate the problem to line 3.  In fact if you employ one of our earler techniques of commenting out line 3 you will see that this has no impact on the error, and is a big clue that line 3 as it is currently written is useless.


NameError
^^^^^^^^^

Name errors almost always mean that you have used a variable before it has a value.  Often NameErrors are simply caused by typos in your code.  They can be hard to spot if you don't have a good eye for catching spelling mistakes.  Other times you may simply mis-remember the name of a variable or even a function you want to call.    You have seen one example of a NameError at the beginning of this section.  Here is another one.  See if you can get this program to run successfully:

.. activecode:: db_ex3_9

    str_time = input("What time is it now?")
    str_wait_time = input("What is the number of nours to wait?")
    time = int(str_time)
    wai_time = int(str_wait_time)

    time_when_alarm_go_off = time + wait_time
    print(time_when_alarm_go_off)

.. reveal:: db_ex39_reveal
    :showtitle: Show me the Solution

    .. admonition:: Solution

        In this example, the student seems to be a fairly bad speller, as there are a number of typos to fix.  The first one is identified as wait_time is not defined on line 6.  Now in this example you can see that there is ``str_wait_time`` on line 2, and  ``wai_time`` on line 4 and ``wait_time`` on line 6.   If you do not have very sharp eyes its easy to miss that there is a typo on line 4.

**Finding Clues**  With name errors one of the best things you can do is use the editor, or browser search function.  Quite often if you search for the exact word in the error message one of two things will happen:

1.  The word you are searching for will appear only once in your code, its also likely that it will be on the right hand side of an assignment statment, or as a parameter to a function.  That should confirm for you that you have a typo somewhere.  If the name in question **is** what you thought it should be then you probably have a typo on the left hand side of an assignment statement on a line before your error message occurs.  Start looking backward at your assignment statements.  In some cases its really nice to leave all the highlighted strings from the search function visible as they will help you very quickly find a line where you might have expected your variable to be highlighted.

2.  The second thing that may happen is that you will be looking directly at a line where you expected the search to find the string in question, but it will not be highlighted.  Most often that will be the typo right there.


Here is another one for you to try:

.. activecode:: db_ex3_10

    n = input("What time is it now (in hours)?")
    n = imt(n)
    m = input("How many hours do you want to wait?")
    m = int(m)
    q = m % 12
    print("The time is now", q)


.. reveal:: db_ex310_reveal
    :showtitle:  Show me the Solution

    .. admonition:: Solution    

        This one is once again a typo, but the typo is not in a variable name, but rather, the name of a function.  The search strategy would help you with this one easily, but there is another clue for you as well.  The editor in the textbook, as well as almost all Python editors in the world provide you with color clues.  Notice that on line 2 the function ``imt`` is not highlighted blue like the word ``int`` on line 4.


And one last bit of code to fix.

.. activecode:: db_ex3_11

    present_time = input("Enter the present timein hours:")
    set_alarm = input("Set the hours for alarm:")
    int (present_time, set_time, alarm_time)
    alarm_time = present_time + set_alarm
    print(alarm_time)

.. reveal:: db_ex311_reveal
    :showtitle: Show me the Solution

    .. admonition:: Solution

        In this example the error message is about ``set_time`` not defined on line 3.  In this case the undefined name is not used in an assignment statement, but is used as a parameter (incorrectly) to a function call.   A search on ``set_time`` reveals that in fact it is only used once in the program.  Did the author mean ``set_alarm``?  If we make that assumption we immediately get another error ``NameError: name 'alarm_time' is not defined on line: 3``.  The variable ``alarm_time`` is defined on line 4, but that does not help us on line 3.  Furthermore we now have to ask the question is this function call ``int(present_time, set_alarm, alarm_time)`` even the correct use of the ``int`` function?  The answer to that is a resounding no.  Let's list all of the things wrong with line 3:

        1.  ``set_time`` is not defined and never used, the author probably meant ``set_alarm``.
        2.  ``alarm_time`` cannot be used as a parameter before it is defined, even on the next line!
        3.  ``int`` can only convert one string to an integer at a time.
        4.  Finally, ``int`` should be used in an assignment statement.  Even if ``int`` was called with the correct number of parameters it would have no real effect.


.. advanced topic!

.. present_time = int(input("Enter the present time(hhmm):"))
.. print type(present_time)

.. min = _ * 60 
.. tot_min = min + [2, 4]
.. print(tot_min)
.. set_hrs = int(input("Enter the hours (hhmm):"))
.. alarm_time = present_time + set_hrs
.. print(alarm_time)


ValueError
^^^^^^^^^^

Value errors occur when you pass a parameter to a function and the function is expecting a certain type, but you pass it a different type.  We can illustrate that with this particular program in two different ways.

.. activecode:: db_ex3_12

   current_time_str = input("What is the current time (in hours 0-23)?")
   current_time_int = int(current_time_str)

   wait_time_str = input("How many hours do you want to wait")
   wait_time_int = int(wait_time_int)

   final_time_int = current_time_int + wait_time_int
   print(final_time_int)


Run the program but instead of typing in anything to the dialog box just click OK.  You should see the following error message:  ``ValueError: invalid literal for int() with base 10: '' on line: 4``   This error is not because you have made a mistake in your program.  Although sometimes we do want to check the user input to make sure its valid, but we don't have all the tools we need for that yet.  The error happens because the user did not give us something we can convert to an integer, instead we gave it an empty value.  Try running the program again.  Now this time enter "ten" instead of the number 10.  You will get a similar error message.

ValueErrors are not always caused by user input error, but in this program that is the case.  We'll look again at ValueErrors again when we get to more complicated programs.  For now it is worth repeating that you need to keep track of the types of your variables, and understand what types your function is expecting.  You can do this by writing comments in your code, or by naming your variables in a way that reminds you of their type.



Summary
--------

* Make sure you take the time to understand error messages.  They can help you a lot.

* The goal is not just to make the program work. The goal is to learn how to make the program work

* ``print`` statements are your friends.  Use them to help you uncover what is **really** happening in your code.

* Work backward from the error.  Many times an error message is caused by something that has happened before it in the program.  Always remember that python evaluates a program top to bottom.


Source
-----------
How to Think Like A Computer Scientist: 
http://interactivepython.org/runestone/static/thinkcspy/Appendices/errorsAndDebug.html
