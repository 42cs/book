.. meta::
   :description: CS 42.
   :keywords: python, CS61A, CS42, computer science

.. raw:: html

   <h1 style="text-align: center">CS42 - Summer 2014</h1>
   <h2 style="text-align: center">Practical Problem Solving with Programming</h2>

.. activecode:: welcome
   :above:
   :autorun:
   :hidecode:

   import turtle
   import random

   def main():
       tList = []
       head = 0
       numTurtles = 10
       drawLogo(turtle.Turtle())

       for i in range(numTurtles):
           nt = turtle.Turtle()   # Make a new turtle, initialize values
           nt.setheading(head)
           nt.pensize(2)
           nt.color(random.randrange(256),random.randrange(256),random.randrange(256))
           nt.speed(7.5)
           nt.tracer(30,0)
           tList.append(nt)       # Add the new turtle to the list
           head = head + 360/numTurtles

       for i in range(100):
           moveTurtles(tList,15,i)

       drawLogo(tList[0])

   def moveTurtles(turtleList,dist,angle):
       for turtle in turtleList:   # Make every turtle on the list do the same actions.
           turtle.forward(dist)
           turtle.right(angle)

   def drawLogo(turtle):
       turtle.up()
       turtle.goto(-100,-25)
       turtle.write("CS 42 ",True,"center","75px Arial")
       turtle.ht()

   main()


First Steps
------------

* Check out the `course outline & goals <CourseInfo/index.html>`_

* Take me to the first topic!  `Reasoning <week1/reasoning.html>`_

* View the :ref:`t_o_c`


Material
----------  

Week 1 - Reasoning & Grit 
~~~~~~~~~~~~~~~~~~~~~~~~~~
This week will get students excited/motivated & make them comfortable reasoning through code (by converting code to english).

.. toctree::
    :maxdepth: 2

    week1/reasoning.rst
    week1/tenacity.rst


Week 2 - Enviroment & Debugging
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We'll make sure that students are comfortable and productive with their text editors and unix. Then we'll practice
using a methodical approach to debugging. 


.. toctree::
    :maxdepth: 2

    week2/ProgrammingTricks.rst
    week2/debug.rst
.. toctree::
    :maxdepth: 1

    week2/DebuggingBook.rst




Week 3 - Git & Collaboration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This week will be timed with the release of the first group project. 



Logistics
~~~~~~~~~~~~
.. toctree::
    :maxdepth: 1

    CourseInfo/index.rst
    Extras/index.rst
    toc.rst



Benefits of this interactive book.
-------------------------------------

* You can experiment with **Active Code** examples right in the page

  * Click Show/Hide Code button
  * Click the Run button

* You can play with code and modify the examples.
* **Interactive questions** make sure that you are on track and help you focus.
* You can highlight text, and take notes in scratch editors

Contact Info
---------------

Sumukh Sridhara
  sumukh @ berkeley.edu

Piazza: http://piazza.com/berkeley/summer2014/cs42

Acknowledgement/Sources
-----------------------
The material under "Background Reading" as well as the artwork above comes directly from 

  How to Think Like A Computer Scientist by Brad Miller, David Ranum.  

Source: http://interactivepython.org/runestone/static/thinkcspy/index.html

The license for the above book is under Creative Commons Attribution-ShareAlike License CC BY-SA as well as the GNU FDL 1.

View the book and the authors here - http://interactivepython.org

This website itself is powered by http://runestoneinteractive.org/

Special Thanks to .... 

