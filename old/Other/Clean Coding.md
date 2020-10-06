Clean coding:
-------------
Chapter 1:
----------
I)Taking responsibility:
	A)Do not harm to function:
		- Fix your bugs.
		- Don't repeat errors.
		
		2. Be certain about your code BEFORE sending it to QA:
			- Don't depend on QA to find bugs, find your bugs.
		
		3. Know your code works:
			- Test it a LOT.
			- Automate your tests.
			- Run unit tests as often as you can.
			- Test Every. Single. Line. Of code.
			- The coverage of your unit tests should be as close as it can to 100%.
			- Design your code to be easy to test.
		
		Test-Driven Development TDD: Write your test first, THEN write your code.
		
		4. Automate the QA testing.
	
	B)Do not harm to structure:
		- The main concept of good software is "Easy to change".
		- So structure is 100% important.
		- If you find changing in it is difficult, refine the design.
		- Do this all the time.
		- This is called "Merciless refactoring"
		- Version control and Good testing are your tools: Use VC to save old structure just in case, and use unit tests with 100% coverage to test new structure.
		- That's how you treat software as a piece of music, always chaning the nits and picks.
		
II) Work ethic:
	- 60 hrs: 40 work, 20 you.
	- Or, give yourself more time to learn.

III) Know your field:
	- Know a lot of knowledge.
	- Constantly increase the knowledge you know.
	- You should know:
		1. All 24 Design patterns of GOF book. 
		2. Have working knowledge in POSA books.
		3. Design principles: SOLID, and component principles.
		4. Methods: XP, Scrum, Lean, Kanban, Waterfall, Structured Analysis and Design, etc.
		5. Disciplines: OOAD, Structured, Continuous Integration, Pair programming, TDD.
		6. Artifacts: UML, DFD, Structured Charts, State transition diagrams, flow charts, decision tables.
		
IV) Continuous learning:
	- Don't stop coding.
	- Don't stop learning new languages.
	- Don't stop learning new techniques.
	- Learn things outside your performance zone.

V) Practice:
	- Treat your SW as a musical instrument, have warm up exercises, have katas, have core exercises, have cooldown exercises. Seriously.

VI) Collaborate, the second best way to learn.
VII) Mentor and Teach, the best way to learn.
VIII) Know your domain, learn the business domain you're helping with your SW.

----------------------------------------------------------------------------------------------------------
Chapter 2: Saying No
----------

----------------------------------------------------------------------------------------------------------
Chapter 3: Saying Yes
----------

----------------------------------------------------------------------------------------------------------
Chapter 4: Coding
----------
First, these are rules not about the code itself, but about the BEHAVIOR of writing code.

Preparedness:
-------------
Code requires concentration, and we live in a world of distractions.

So, to write code efficiently:

1- Do not code if you are tired or distracted. Take a break then come back.

2- Do not stay late to code, do not write code at 3 a.m.

3- Do not code if you have axieties in your head, set time at home to at least think about them and address them. If not at home then at the office, then try to go back to coding.

4- The "Zone" when you are so focused you feel productive and want to write more code? Avoid that, take a walk then come back. But get in the zone when you practice.

5- It's preferred not to code while listening to music, it may help you get in the Zone.

6- If interruptions happen, be friendly, and make sure you record the context you were in so that when you come back you don't lose concentration.

7- When you have a mind block and can't write code, review your sleep, food, etc.

Get a pair partner, sit next to another coder, you'll start to code in no time.

8- Creativity, intelligence, and energy, are limited resources, they will eventually deplete, and when they do, no matte how long you stay at the office, you won't solve any problem.

When you are stuck or tired, disengage for a while. (i.e. drive home, take a shower, etc.)

Learn your creativity patterns and your energy spikes and take advantage of them.

9- Make sure that your sleep, health, and lifestyle are tuned so that you can put EIGHT GOOD hours of code per day.

10- If you're late (Which will happen), early dedication and transparency.

11- Regularly measure progress and come up with best-case end-date, normal case, and worse case.

Present these numbers to the manager and stakeholders.

Update the numbers daily!

12- If your manager tells you to meet the deadline and rushes you, stick to your estimates. NEVER. RUSH. OR. CHANGE. YOUR. MIND!

You can't make yourself code faster and solve problems faster. Period.

13- DO NOT WORK OVERTIME UNLESS: it's less than two weeks, you can afford it, and your boss has a plan in case the overtime fails.

If the last one isn't achieved, do NOT work overtime whatsoever.

14- Never. Ever. Say you're done when you are not.

15- To be sure you're "Done", make business analysts and testers create acceptance tests written in a testing language, can be understood by stakeholders, and should run frequently.

16- Ask for help.

17- Help others. If you sense someone is in trouble, try and help them.

Assert that you have some time for yourself, and some other time you're available for help.

Spend about an hour with that person.

18- Accept help from others, 







----------------------------------------------------------------------------------------------------------
Chapter 5: TDD
----------
TDD is adopted by Agile and Scrum emthods.

Why TDD?
--------
1. TDD lets you write automated tests with high coverage,
2. So you can test your code every slight change,
3. So you make sure things work.

TDD rules:
----------
1. DO NOT write any production code BEFORE writing a failed unit test.

2. Expand your unit test until it fails and DO NOT ADD anything until you fix your production code so that it succeeds (even not compiling counts as failing).

3. Once you write production code that passes the test, STOP and go back to writing unit tests.

So unit tests act as milestones for you.

Why?
----
1. you will have SO MUCH reusable unit tests with very high coverage.

2. much much less issues post-deployment.

3. you can use your unit tests on existing projects to be sure any change you do isn't making a logical error.

4. when documenting, each unit test is an example code that describes how the system is used, and for everything that is done there will be a unit test for it, unit tests are documentation.

5. when writing tests BEFORE writing the code, you force yourself to decouple every single piece of code, you enforce Good Design.



----------------------------------------------------------------------------------------------------------
Chapter 6: Practicing
----------
*** HDD? ***
Kata: sequence of movements that simulates one side of combat.
	- Goal: to teach your body to make these movements perfectly and assemble them into fluid.
	
Programming kata: set of keystrokes and mouse movements to solve some programming problem.
	- Goal: not to solve it (since you already know the answer), but to practice movements and decisions involved in solving it.
	
	
Repeat the kata overand over.
While repeating, you may discover improvements to make.

This is helpful to learn hot keys, etc.

And to learn disciplines like TDD and CI.

But most importantly, to drive common problem/solution pairs into your subconscious.

Practice katas on:
	- kata.softwarecraftsmanship.org
	- codekata.
	
Try these:
	- Bowling game.
	- Prime factors.
	- Word wrap.

Wasa: two-man kata, you swap roles as attacker and defender.

Programming WASA: ping-pong:
	- Choose a problem,
	- One coder writes a unit test,
	- One coder writes a solution that must pass the unit test.
	
	Then they reverse roles.
	
Why?
- If old problem, both coders test their memorization of the kata.
- If new problem, tester can put constraints to force programmer to change algorithm, and it gets more fun.

Randori: Random combat scenarios.

Programming randori: same as ping-pong, but with many people in circle, 
	- The problem is displayed,
	- One person writes a unit test,
	- The next person passes the test, then writes the next test, 
	- and so on.
	
Why?
- Wasa on large-scale,
- Learn how other people solve the same problems.


How to broaden experience:
--------------------------
Contribute to open-source. (In stuff that's different from your job)


Summary of chapter 4:
---------------------
How to practice:
	- Do Katas A LOT (every day before programming)
	- Do Wasas with another person (Or yourself if noone is involved).
	- Do Randoris with other people.
	- Contribute to open-source.
	
-------------------------------------------------------
Chapter 7: Acceptance testing:
------------------------------
Customers usually know what they want clearly when they see the requirements that they communicated actually run - because usually it's not what they want.

