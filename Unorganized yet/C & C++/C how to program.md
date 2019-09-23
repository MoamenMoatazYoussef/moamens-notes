C how to program:
-----------------
Intro:
------
Computers can be divided into:
1. Input units.
2. Output units.
3. Memory.
4. ALU.
5. CPU
6. Secondary storage, or disks.

Characters: C supports 16-bit unicode, chars take two bytes.

A subset of 16-bit unicode is ASCII.

Compiler: compiles high-level language into machine code.

Interpreter: directly executes scripting languages (Python, js, etc.), slower than compilers but faster in terms of not needing compilations.

Interpreters are better in internet scripting.
------------------------------
C is hardware independent if designed correctly.

C is used for performance sytstems e.g. OS, embedded, RT, communication.

To learn C, you should:
1. Learn the language. (C language)
2. Learn the library. (Standard C library)

Using C standard library makes it more portable and better performance.

C programs go through:
1. Edit (writing the code).
2. Preprocess (compiler directives).
3. Compile (produce machine code file or object code).
4. Link (getting referenced libraries and stuff and linking it to the object code and produces an executable image).
5. Load (loading the program in memory).
6. Execute.

A) write code in a file and save it as .c

preprocessing is done automatically using compiler directives.

B) Compile AND link it using cmd:
>> gcc file.c

produces a file "a.out".

Loading is automatically done when executing.

C) Loading and executing the program by cmd:
>> ./a.out

----------------------------
Standard io and errors:
-----------------------

input usually using library stdin.

output usually using library stdout.

error messages usually using library stderr.

It's common to route stdout to an output device and stderr to a screen.

*** Running a C program on Win, Linux, Mac OS ***

*** Operating systems ***

** The internet and WWW ***

-----------------------

Key terms in SW:
- Ajax: helps internet-based apps perform like desktop apps.

- Agile sw dev: a methodology of making SW get implemented faster and with fewer resources.

- Refactoring: making code clearer and easier to maintain while preserving functionality.

- Design patterns: proven architectures to construct flexible maintainable OOP software.

- LAMP: stack consisting of Linux, Apache, MySql, PHP/Perl/Python.

*** Keeping up-to-date with info tech ***

==================================================================
Chapter 2: intro to C.
----------------------
#include //any line beginning with # is a preprocessor line, processed BEFORE the compiler starts.


int main(void) { //The entry point
	printf("string"); //prints a string

}
/////////////

>> GPP: comment before every function describing what it does.

f in printf stands for "formatted".

When compiler finds \ it looks at next char and combines them into escape character, then executes it.

>> GPP: add comment to the line containing the end of the function, stating it's the end.

When we use a function that is not part of the language, the compiler doesn't know where it is, but provides space in the object code for it. The linked knows where is it, locates it, and inserts the proper call in the object code. Then the executable is generated and is complete.

///////////////////
#include <stdio.h>

int main(void) 
{
	int i1;
	int i2;
	
	scanf("%d", &i1);
	scanf("%d", &i2);
	printf("sum is %d", i1+i2);
}
///////////////////

Prompt: a message telling the user to do something.

scanf: reads two params:
1. "%d": a format control string, specifies if data is integer, float, hexa, etc.

% is treated like \ but next char is conversion specifier.

2. &i1: the & is address operator, this tells the compiler to locate this address and store the value there.

Forgetting the & will result in a segmentation fault error because the user is trying to access memory which isn't allocated or doesn't have access auth.

...

When you assign a value to a variable, you overwrite the previous value, this is called a "destructive process".

On the contrary, when you read a value from a variable, you don't replace it, it's a "nondestructive process"

Remember that integer division returns an integer.

Precedence of operators in C:
2: Mul, dive, and remainder, left to right.
1: (): left to right.
2: *, /, %: left to right.
3: +, -: left to right.
4: <, <=, >, >=: left to right.
5: ==, !=: left to right.
6: =: right to left.

----------------------
Statements do one of two things:
1. Actions: calc, i/o, etc.
2. Decisions.

Note: in C, any expression evaluating to 0 can be viewed as condition evaluating to false, and if non-zero then true.

btw we can do this:
scanf("%d%d", &i1, &i2);

and write in the input:
>> 12 22
----------------------
Secure C programming:
--------------------
Some guidelines that help program in C without leaving weak points to attackers:

- No single-argument printf. Instead use puts:
///
puts("Hello World!");
///

puts adds a new line automatically.

If you don't want a new line, use this:
///
printf("%s", "Hello World!");
///

We'll know why this matters later.


==================================================
Exercises chapter 2:
--------------------
2.1:
a) main
b) { }
c) ;
d) printf
e) newline
f) scanf
g) %d
h) destructive
i) non-destructive
j) if

2.2:
a) false, puts/at the cursor position
b) false, printf/ignore
c) true
d) true
e) true
f) false, different
g) false, definition of variable before use
h) false, scanf/not
i) false
j) false, */% then +-
k) false, can be in one printf with three \n s

2.3:
a) int c, thisVariable, q7, number;
b) printf("Please enter an integer: ");
c) scanf("%d", &a);
d) if(number != 7)
	printf("..%d..7", number);
e) printf("");
f) printf("\n program");
g) printf("this\nis\na\nc\nprogram");
h) printf("this\tis\ta\tc\tprogram");

2.4:
a) //the program will calculate product of three integers
b) int x, y, z, result;
c) printf("Enter three ints: ");
d) scanf("%d%d%d", &x, &y, &z);
e) result = x * y * z;
f) printf("the product is: %d", result);

2.5:

#include <stdio.h>

int main(void) 
{
	int x, y, z, result;
	printf("Enter three integers: ");
	scanf("%d%d%d", &x, &y, &z);
	result = x * y * z;
	printf("The product is: %d", result);
}

2.6:
a) erase the &
b) & before number2
c) no ; after if
d) >= not =>
==============================================================================
Chapter 3:
----------
- Algorithm: specific actions in specific order to solve a specific problem.

- Psuedocode: consists of action statements e.g. input, output, calculate, compare, etc. NOT definitions. But it's okay to write them if you want.

- Transfer of control: making the next statement to be executed NOT the next in order.

Previously, we had the goto statement, but that made code so complicated to track.

Now, we have sequence, selection, and repitition structures, these can replace goto statements completely.

- Flowcharts.

Selection statements:
---------------------
if
if else (if else if)
switch
?: (the only ternary operator)
///
condition ? true stmt : false stmt;
///

Repitition statements:
----------------------
while
do while
for


>> GPP: type the {} BEFORE typing the statements inside them.
--------------------------
Formulating algorithms:
-----------------------
1. Counter-controlled algorithms (number of repitition is KNOWN before the loop begins)

2. Sentinel-controlled repitition (number of iterations is NOT known)
	- Here, we may use a sentinel value (signal value, dummy value) to endicate end of data entry e.g. -1.
	
When using this, remind the user of the dummy value in the prompt.

>> Problem Modelling Method: Top-down refinement, begin from the general description.

First refinement: then divide the top into a series of smaller tasks and list them.

Second refinement: now specify the variables, the totals and counters, etc.

You continue refinement until you're sure you can translate the pseudocode into actual code.


Casting:
--------
When ints divide, they may produce float, this is truncated BEFORE assignment to int value, so we can't just assign to float variable.

We need to cast it explicitly.

Otherwise, this:
///
float x = (float)y + (int)z;
///
is called casting z to float implicitly.

- Specifying decimal point approximation limit:
///
printf("%.2f", 3.446); //prints 3.45
printf("%.1f", 3.446); //prints 3.4
///

- DO NOT use floats in comparison, precision may mess things up.

3. Nested control statements.

>> The most difficult part of solving a problem is developing the algorithm. Once that's done the syntax is straightforward.

Precedence until now:
----------------------
c++ and c-- (r to l).
+, -, (cast), ++c, --c "unary ops" (r to l).
*, /, % (l to r).
+, - (l to r).
comparison (l to r).
== and != (l to r)
?: (r to l).
=, +=, -=, etc. (r to l).


Secure C programming 2:
-----------------------
Arithmetic overflow:
	>> a = b + c;
	- if b + c is greater than int value, this is called Arithmetic overflow.
	
	- This could leave the system open to attacks.
	
	- The max and min values of int are INT_MAX and INT_MIN, stored in <limits.h>
	
	- Solution: make sure they won't overflow BEFORE you perform arithmetic calcs.
	
	- We can search for INT32-C guideline in cert.org.
	
Unsigned integers:
	- Unsigned ints can have about twice as large of a value range as signed int, defined in UINT_MAX in <limits.h>
	
scanf_s and printf_s
	- 
-------------
General tips:
-------------
- If counter variable, declare it unsigned.
- You can divide many programs into three parts: initialization, input processing, and calculation of output.

==================================================================
Chapter 4:
----------
Loops:
------
- Comma-separated lists of expressions in for loops:
	We can do this:
	///
	for(int i = 0 , j = 0; i <=10; i++, j+=2);
	///

	
- char g = getchar(); // get one char

///
a = b = c = 0; //first c = 0, the n b = c, then a = b.
///

Using getchar inserts newline charactrer in input, so remember to check for it in switch-case statements.


>> GPP: Don't merge stuff into for loop's header.
	
>> GPP: don't exceed 3 levels of nesting.

>> GPP: always use integers for counters.

>>