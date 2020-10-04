Math for CS:
------------
In CS, we may want to prove if a program does what it should do, because programs are full of bugs and both researchers and programmers want to prove these programs correct.

So, developing mathematical models to verify programs is a powerful thing for a computer scientist, and for that, we need to understand what a proof is.

Chapter 1: What is a proof:
---------------------------
Proposition:
------------
Simply it's a claim, a statement that can be true or false.

e.g. "for every non-negative integer n, the value of n**2 + n + 41 is prime".

Now, if we name this function:
	p(n) = n**2 + n + 41
And get p(0, 1, ... 39), all will be prime.
BUT, p(40) = 41 * 41 != prime!
We can say that this proposition is NOT true for all non-negative integers.

But, we can come up with polynomial expressions that get prime numbers up to much more than 41, and we can't just keep checking all of them i.e. we can't check a claim about an infinite set by checking a finite sample of its elements.

Perdicate:
----------
It's a proposition that depends on the value of some variables to be true.

e.g. p(n) = "n is a perfect square"

You can't say p(n) is true or false unless you know what n is.

perdicates take the value of TRUE or FALSE only, unlike propositions which can take numerical values.

The axiomic method to prove:
----------------------------
Axioms: propositions that are just accepted as true e.g. between every pair of points there's a straight line.

A proof: is a sequence of logical deductions from axioms and previously proved statements, whose conclusion at the end is the proposition in question.

When a preposition is proved:
- If it's very Important and has a significant impact, it's called a "Theorem".
- If it stems from a theorem, it's called a "Corollary".
- If it's used to prove other prepositions, it's called a "lemma".

Axioms:
-------
*** ZFC axioms ***

They are axioms that justify the foundations of maths, and they are like writing code in binary, proving 2 + 2 = 4 requires more than 20,000 steps!

So, we're going to pick some axioms to work with since they are useful for CS.

Sometimes, you may take little facts in the middle of proofs as axioms, and sometimes you may need to prove them.

Be upfront about what you're assuming.

Logical deductions:
-------------------
They are used to prove new prepositions using old proven ones, a.k.a. "Inference rules".

Inference rule 0 (IR0 -my notation-): a proof of P, with a proof that P implies Q, is a proof of Q.

This is called "modus ponens".

Notation for writing inference rules:
	(P, P implies Q) / Q
	
The stuff above the line "Antecedents", when they are proved, then the thing beloe the line, the "consequence", is proved.

Inference rules should be sound. We'll use them to prove stuff.

Some natural inference rules that are sound:
- IR1:(P implies Q, Q implies R) / (P implies R)
- IR2: (Not P implies Not Q) / Q implies P

But this:
	(Not P implies Not Q) / P implies Q
is NOT sound.

Patterns of proof:
------------------
Because a proof can be any logical deduction that leads to a proposition, there are some standard templates so that we can get an outline for the proof.

Proving an implication:
-----------------------
Implication: a proposition in the form IF THEN

How to prove that "if P then Q":
Method 1:
	- Assume P.
	- Show that Q logically follows.
	
e.g. for 0 <= x <= 2, prove that:
	-x**3 + 4x + 1 > 0
	
Proof:
	Assume 0 <= x <= 2
	Then x, 2 + x, 2 - x are all > 0.	
	Therefore, product of these is >0.
	Therefore, adding 1 to the product > 0.
		x(2 + x)(2 - x) + 1 > 0
	Multiplying this:
		-x**3 + 4x + 1 > 0
			Q.E.D.
			
Method 2:
	- Prove that Not(Q) implies Not(P).
	
e.g. if r is irrational, then sqrt(r) is irrational.

Proof:
	We try to prove the contrapositive: if sqrt(r) is rational then r is rational.
	Assume that:
		sqrt(r) = m / n
	where m and n are integers.
	squaring both sides:
		r = m**2 / n**2
	since m**2 and n**2 are integers, therefore r is rational.
	
	Therefore, if r is irrational, then sqrt(r) is also irrational.
		Q.E.D.
		
Proving "iff":
--------------
If and only if P then Q.
This implication usually means that both are equivalent i.e. P implies Q and Q implies P, so:

Method 1:
	- Prove that P implies Q. (Using previous methods)
	- Then prove that Q implies P.
		
Method 2:
	- Prove that P IFF something, then something IFF another thing, then that other thing IFF yet another thing ... until something IFF Q.
	- It's like a chain of IFFs.
	
Proof by cases:
---------------
Breaking the proof into cases and proving each one.

Break the proposition into cases that might occur, then try to show that the proposition holds for each case.

Proof by contradiction:
-----------------------
If this proposition was false, another proven fact would not be true.

Method:
	- Assume P is false.
	- Deduce something known to be false.
	- Write "this is a contradiction, therefore P must be true"
	
How to write a good proofs:
---------------------------
1) State your plan.
2) Keep a linear flow.
3) A proof is an essay, not a calculation.
4) Use minimum symbols.
5) Revise and simplify.
6) Introduce notation thoughfully, define it, and generally try to lessen new notation.
7) Design your proof just as you design your program, prove the small lemmas first separately (like defining functions), then use them (like calling functions).
8) Things that are obvious should be revised "Are they obvious to you, or the reader?".
9) Conclude after proving the claim, state why this matters, why it follows, and what this means.

