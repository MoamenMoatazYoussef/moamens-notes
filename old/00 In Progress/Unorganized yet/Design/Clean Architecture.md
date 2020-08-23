Clean Architecture:
===================
A) Paradigms:
-------------
Paradigms of programming are essentially ways to structure our programs and languages so that we AVOID some stuff, paradigms tell us what NOT to do.

1- Structured:
--------------
The idea is that any program can be constructed from three things:
- Sequence.
- Selection. (if)
- Iteration. (loops)
As proven by Bohm and Jacopini.

Then Dijkstra created these to avoid using goto statements altogether and allow for programming to be treated as mathematics in a way, through applying DnC like mathematical proofs.

Structured programming was born.

Then Dijkstra proved mathematically that sequential statements, selection, and iteration are siffuce.

Structured programming allows modules to be decomposed into provable units recursively i.e. you can take a big problem and decompose it into high-level functions which can be decomposed into lower-level functions which can be .... and so on, until you reach the previously mentioned control statements.

Formal proofs are an appropriate way to produce high-quality software. THe "Euclidean hierarchy of theorems" dijkstra dreamed of never came to light, but it's a way.

On the other hand, scientific methods tend to rely on proving hypotheses WRONG, to debunk them, if they're not proven wrong for long enough they are considered right, that the principle of Testing.

--------------------------------------------
2- OOP:
-------
What is OO?
It has many definitions, but we'll focus on defining the three pillars of OO:

a) Encapsulation:
First of all, encapsulation is merely hiding data and coupling data with functions where some of the functions are used to access data.

That can be done in structs in C, we can define headers of struct and related functions in .h file, and have the real implementation in the .c file.
//// point.h
struct Point;
struct Point* makePoint(double x, double y);
double distance (struct Point *p1, struct Point *p2);
////

//// point.c
#include "point.h"
#include <stdlib.h>
#include <math.h>

struct Point {
  double x,y;
};

struct Point* makepoint(double x, double y) {
  struct Point* p = malloc(sizeof(struct Point));
  p->x = x;
  p->y = y;
  return p;
}

double distance(struct Point* p1, struct Point* p2) {
  double dx = p1->x - p2->x;
  double dy = p1->y - p2->y;
  return sqrt(dx*dx+dy*dy);
}
////

But when C++ decided that member vars should be in the .h file, encapsulation was broken (The compiler can't still access the data, but the client now knows this data exists and can access them using the dot operator!).
//// point.h
class Point {
public:
  Point(double x, double y);
  double distance(const Point& p) const;

private:
  double x;
  double y;
};
////

//// point.cc
#include "point.h"
#include <math.h>

Point::Point(double x, double y)
: x(x), y(y)
{}

double Point::distance(const Point& p) const {
  double dx = x-p.x;
  double dy = y-p.y;
  return sqrt(dx*dx + dy*dy);
}
////

The solution? Access modifiers.

Then, Java and C# abolished this concept and now classes are put in one file, so no encapsulation is ensued!

So, contrary to what people believe, encapsulation is not part of the definition of OOP.

Ironically, encapsulation in C was perfect.

b) Inheritance:
Once again, that isn't a new concept, inheritance is redeclaring some functions and vars in a different scope, which can be easily implemented in C.

In fact, this is the way C++ implements single inheritance!

OO languages didn't introduce inheritance, but it made it much more convenient to use, it removed necessary casting of structs and made different implementations to make multiple inheritance.

c) Polymorphism:
Again, we can do polymorphism in C.
////
#include <stdio.h>

void copy() {
  int c;
  while ((c=getchar()) != EOF)
    putchar(c);
}
////

getchar and putchar read/write from/to STDIN/STDOUT.

But both STDIN and STDOUT are polymorphic, they have different implementations for each input/output device.

How is that achieved?
Every IO device driver should provide at least five operations: Open, Close, Read, Write, Seek.

So we have:
////
struct FILE {
  void (*open)(char* name, int mode);
  void (*close)();
  int (*read)();
  void (*write)(char);
  void (*seek)(long index, int mode);
};
////

And getchar calls the function pointed to by "read" in the data structure FILE pointed to by STDIN.

And this is the bases of ALL polymorphism in OO.
That's the idea of the vtable and vptr in C++.

OO didn't introduce polymorphisn either, BUT OO made it SO MUCH EASIER AND SAFER to use than pointers to functions.

That's why we said OO imposes discipline on indirect transfer of control.

The power of polymorphism is that it decoupled programs from device drivers, it's the main pillar of reusability.
--------------------------------------------
3- Functional:
--------------


----------------------------------------------------------------------------------------
B) SOLID:
---------
1- SRP: Single Responsibility Principle:
----------------------------------------
Contrary to popular belief, this does NOT mean an object should do ONE thing. (There's a principle for that but not the SRP).

"A module should be responsible to one and only one actor".

Where an actor: a group of people whose tasks are related to achieving one objective?

Module: in the simplest definition it's a source file, a set of functions and DSs.

What happens if we don't follow?
--------------------------------
1- Accidental duplication: if we put many functions related to one entity but in different business areas, each business area may have a change that is not necessarily common with other business areas, so we may need to copy some code and change it.

So, separate the code that different actors depend on.

2- Merges: if two business areas require two different changes, two developers work on ONE code, and merging will have conflicts and need to change, also merges are generally risks taken so both business areas are at risk.

So, separate the code that different actors depend on.

How to use the principle:
-------------------------
1- Separate data from functions, data that is common to business areas are put in one class, but functions are separated into their own classes.

2- But now instead of one class we have three :( solution is to use the facade pattern.

3- If we want important business logic to be closer to the data, we put the most important functions inside the main class, then separate the rest and use the main class as the Facade for the rest.

----------------------------------------------------------------------------------------
2- OCP: Open-closed principle:
------------------------------
It means: a software artifact should be able to  be extended, but not modified.

This is used to design classes and modules, but it can be applied for things bigger than arch components.

For example:
If we have a web page that displays a financial summary, but the stakeholders want that summary to be printed on paper.

If we applied the SRP principle we'll have three separate components:
	- One having the financial data.
	- One that does the calculation for the summary.
	- And one that presents the summary on the web page.
	
Now, we'll need to add new code to print the paper, but where do we add it and how do we add it? And how much code would we need to MODIFY?

Well, let us partition the processes into classes, then group these classes into components, look at this configuration:

"file:///C:/Users/Mo'men/AppData/Local/Temp/ppkdsgdd.ha0/images/00030.jpeg"

If component A should be protected from changes in component B, then B should depend on A.

In our example we want to protect the controller from changes in the presenters, therefore we want the presenters to depend on the controllers.

We also want the interactor to be protected from changes in anything.
Why? because it has the core business rules, it does the central thing of the software, the others perform peripheral concerns.

The controller does the central thing RELATIVE to the presenters, that's why we want to only protect it from the presenters.

Notice that we use the SRP to separate modules, then give priorities to each module, then decide which module should be protected from which module, then we contruct our dependencies based on the OCP principle.

----------------------------------------------------------------------------------------
3- LSP: Liskov Substitution principle:
--------------------------------------
If we have a base class and a child class, we should be able to substitute any instance of the base class with an instance of the child class at ANY TIME.

Why? well the child class should be extending the base class, not violating it.

Consider this:
////
public interface license {
	int calcFee();
}

public class A implements license {
	int calcFee() {
		return 10;
	}
}

public class B implements license {
	int calcFee() {
		return 20;
	}
}
////

This obeys the LSP principle.

But, consider this:
////
public class Rectangle {
	void setHeight(int h)
	{...}
	void setWidth(int w)
	{...}
}

public class Square extends Rectangle {
	void setSide(int s) {
		setHeight(s);
		setWidth(s);
	}
}
////

Now, that violates the LSP because if we substitute the reactangle object with a square object, we can get some errors from the area since whenever we change height or width the side of the square changes.

////
Rectangle r = ...;
r.setHeight(5);
r.setWidth(2);
assert(r.area == 10);
////

That code would fail if we use a Square.

Example of LSP in arch:
-----------------------
If we have an app like uber that chooses between taxi companies and contacts a certain taxi car, let's say it communicates using REST API, for example the car chosen has the web service:

	cab.com/driver/bob
	
And the app does this:
	cab.com/driver/bob/pickupAddress/24 Maple st.
	/pickupTime/153
	/destination/ORD
	
What if another company comes in with a new taxi where it requires only one attribute: /dest

We'll need to add a special case into the app's code, which indicates a problem in the architecture.

Therefore, we should use the LSP in here somehow.

So in summary: don't inherit from a class unless you implement everything it does AND extend it without violations.

----------------------------------------------------------------------------------------
4- ISP: Inteface Segregation Principle:
------------------------------
If we have a class with three functions, and three users each use one function, if we change the first function, the class will have to be recompiled and redeployed for ALL USERS.

To solve this problem, we define three interfaces, one per user, and each has the prototype of ONLY functions relevant to that user.

Now when we change the class, other users won't bother since interfaces are the same.

When you use import, using, or include, these declarations create dependencies that force recompilation and redeployment.

That is in statically typed languages (Java, C++, etc.), but in dynamically typed languages (Ruby, Python, etc.) declarations are inferred at runtime, so recompilation and redeployment has no dependencies.

But the idea in architecture is:
"Don't depend on modules that contain more than what you need".

If we have system S, and the developer decides to use interface F into S, and F depends on database D, which contains features that S doesn't need.

If any of these features change, D will force F to be redeployed and in turn S.
Also, a failure in one of these unwanted features may cause F or even S to fail.

----------------------------------------------------------------------------------------
5- DIP: Dependency Inversion principle:
---------------------------------------
To make flexible system, make the source code dependencies refer to abstractions, not concretes.

Abstraction: a module where function PROTOTYPES exist.

Concrete: a module where function IMPLEMENTATIONS exist.

This is why when we include a file in C we only include the header file i.e. the one containing prototypes only.

We refer to modules containing interfaces, abstract classes, etc.

This is true for both statically and dynamically typed langs, but in dynamically langs it may be harder.

This principle doesn't apply always since you have to refer to the concrete String class when using it.
But the string class is VERY stable and rarely changes, so you don't have to worry about changes here.

Why?
----
Changes in concretes may not result in changes in abstractions, but not vv.

So interfaces and abstractions are less volatile than concretes.

The more stable interfaces are, the better the design of the software.

Practices for DIP:
------------------
- Don't refer to volatile concrete classes, refer to abstracts, and use Abstract Factories for them.
- Don't inherit from volatile concrete classes.
- Don't override concrete functions: when you override a function you don't eliminate its dependencies, you inherit them, so make the function abstract and create multiple implementations.

Creation of volatile objects:
-----------------------------
Creating a volatile object creates a dependency on the concrete definition of the object, so use Abstract Factory to create these objects.

i.e. if class A uses interface I for class B, and we want to create an object of B, we can't go to B directly cuz that's a dependency and we're using I for that.

So, we create another class B_Factory which creates an object B and returns it, and an interface I_Factory, and let A use I_Factory to create objects, without depending on B's definition, Tada!

Also we still don't depend on the concrete factory.

Concretes in your diagram should point to interfaces in one direction (no circular dependency) like this:
file:///C:/Users/Mo'men/AppData/Local/Temp/g2k5jhvp.32c/images/00040.jpeg

Draw a curved line across the arrows to interfaces and verify this, then this line divides stuff into components: one abstract and one concrete.

The abstract will contain high-level business rules, the concrete contains the implementation stuff.

The flow of control is inversed by the interfaces since now interfaces depend on concretes, which is why the principle gets its name.

Note that still factory depends on B, but you can't eliminate all concrete dependencies, you're trying to limit them and separate them from the system as much as possible.

The main function will always contain the concrete component.