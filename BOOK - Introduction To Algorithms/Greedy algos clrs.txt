Greedy clrs section 4: (and 5)
----------------------
Matroids:
---------
Problems that greedy algorithms always yield optimal solutions.
They have a certain structure called Matroid.

They don't include all greedy problems e.g. activity selection and huffman coding neither is a matroid.

Matroid; An ordered pair M = (S, I) Where:
	- S is a finite set.
	- I is a nonempty family of subsets of S, called independent subsets of S.
	- {} is a member of I.
	- If we have B belongs to I, and A is a subset of B, then A belongs to I. (I is called a hereditary).
	- If A belongs to I, B belongs to I, |A| < |B|, then there's an element x that belongs to (B - A) such that A u {x} belongs to I this is called the Exchange Property.
	- 