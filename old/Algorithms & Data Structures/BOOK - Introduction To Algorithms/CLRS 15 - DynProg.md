Dynamic Programming:
--------------------
DnC applies when subproblems are disjoint.
DynProg applies when subproblems overlap and there are shared subsubproblems.
Dynprog's approach is to solve these subsubproblems and save their answers in a table, retrieving them when used again in the overlapping stuff.

Dynprog is used in optimization problems (Min or max).

Steps of solving DynProg algorithms:
------------------------------------
1- Characterize the structure of an optimal solution.
2- Define the value of that solution recursively.
3- Compute that value in a bottom-up fashion.
4- Combine the info to make the optimal solution.

E.g. Rods:
----------
We want to cut long steel rods into shorter rods,
each short rod has a price depending on its length in inches,
we want to calculate the max revenue we can get,
and how can we cut the rods in a way to achieve that max revenue?

For example if we have a rod of length 4 inches we can cut it into (3, 1) or (2, 2) or (0, 4) or (2, 1, 1) or (1, 1, 1, 1)
length: 1, 2, 3, 4
price:  1, 5, 8, 9
(1, 1, 1, 1) = 1 + 1 + 1 + 1 = 4
(2, 1, 1)	 = 5 + 1 + 1 	 = 7
(1, 2, 1)					 = 7
(1, 1, 2)					 = 7
(3, 1) 	 	 = 8 + 1 		 = 9
(1, 3)		 				 = 9
(4, 0) 	 	 = 9 + 0 		 = 9
(2, 2) 	 	 = 5 + 5 		 = 10 ==> Maximum price ==> Optimal solution.

How do these solutions overlap?
	If we cut a part of 3, we automatically determine the length of the remaining part or at least the limit of length we can get with the remaining part.
	
Generally, we can cut a rod of length n into 2**(n-1) different ways.
Step 1: characterize the optimal solution structure:
	The optimal solution would be a decomposition of the rod such that we get the max revenue i.e. if we cut the rod into:
		n = i1 + i2 + ... + ik
	pieces, then we would get max revenue:
		r = p1 + p2 + ... + pk
		
Step 2: define the value of the solution recursively:
	So, for n >= 1 the solution is:
		r = max(pn, r1 + r[n-1], r2 + r[n-2] + ... + r[n-1] + r1)
			
	Where:		
		pn: 		 no cutting.
		r1 + r[n-1], r2 + r[n-2] ... : correspond to all the different ways to cut the rod into two pieces, r[k] and r[n-k], then for r[k] we have:
			pk = max(pk, r1 + r[k-1], ... , r[k-1] + r1)
		The same thing, and the same thing for r[n-k]
		That's the DnC element of it.
	
	Notice that if we want to solve this problem, we may need to cut the rod into more than two pieces which is equivalent to cutting it into two pieces then cutting those two pieces into more pieces and so on.
	In other words, the subproblems generated from the initial cut is a smaller instance of the bigger one.
	Also, cutting each piece optimally generates the optimal solution of our problem.
	
	This is called the "Optimal Substructure Property".

	We can also express the solution in terms of cutting into two pieces where one piece is FINAL and the other can be cut, i.e.:
		pn = max[1 <= i <= n](pi + r[n-i])
	That way we solve only ONE subproblem.
	
Step 3: compute value in bottom-up fashion:
	Before we do this, let's see a straightforward implementation of the SECOND expression:
	
	Algorithm:
	////
	cutRod(p, n)
	{
		if n==0;
			return 0;
		q = -INF;
		for(i = 1; i <= n; i++)
		{
			q = max(q, p[i] + cutRod(p, n - i));
		}
		return q;
	}
	////
	
	That works, functionally, but with a large n it would take a long time to run..
	
	Why is this algorithm not efficient?
	Because it's called recursively with the same parameter values over and over, some subsubproblems and subproblems are solved repeatedly.
	
	Time complexity:
		T(n) = 1 + sum[0 -> n-1](T(i))
		
	Solving this would turn out to be:
		T(n) = 2**n
		
	Also, think about it, this algorithm considers all 2**(n-1) possible values to get the maximum.
	
So how do we solve that?
	The idea is, instead of resolving subproblems, we store their results so that if we want them we go and get them.
	Essentially we trade memory for time, and it usually transfers exponential time to polynomial time, which is a BIG improvelemt.
	
	This has two approaches:
		Top-down approach: solve subproblems and store them in an array or hash table, then when solving a subproblem look up the solution BEFORE you compute it.
		Bottom-up approach: this depends on the fact that some subproblems depend on solving smaller subproblems, so we sort the problems by size and solve them bottom-up.
		
	Algorithm (Top-down):
	////
	cutRodTopDown(p, n)
	{
		r = new array[n];
		for(i = 0 ; i < n ; i++)
		{
			r[i] = -INF;
		}
		return cutRodTopDownAux(p, n, r);
	{
	
	cutRodTopDownAux(p, n, r)
	{
		if r[n] > 0 {
			return r[n];
		}
		if n == 0 {
			q = 0;
		} else {
			q = -INF;
			for(i = 1; i <= n; i++)
			{
				q = max(q, p[i] + cutRodTopDownAux(p, n-i, r));
			}
		}
		r[n] = q;
		return q;
	}
	////
	
	Notice that it's so similar, we just check first, then save later.
	Time complexity:
		T(n) = O(n**2)
	
	Algorithm (bottom-up):
	////
	r = new array[n];
	r[0] = 0;
	for(i = 1 ; i <= n ; i++)
	{
		q = -INF;
		for(j = 1; j <= i; j++)
		{
			q = max(q, p[i] + r[j - i]);
		}
		r[j] = q;
	}
	return r[n];
	////
	Time complexity:
		T(n) = O(n**2);
		
Subproblem graph:
-----------------
It's a graph that represents how subproblems depend on each other.
It's like a small recursion tree showing which subproblems depend on each other.
Top-down dynprog is like a DFS of the graph.

We can use it to guess the running time, as the time to compute a subproblem depends on the number of edges of the vertex corresponding to the subproblem, so the running time is linear in terms of vertices and edges.

Step 4: return the choice that made us achieve the optimal solution:
	Algorithm:
	////
	r = new array[n];
	s = new array[n];
	r[0] = 0;
	for(i = 1 ; i <= n ; i++)
	{
		q = -INF;
		for(j = 1; j <= i; j++)
		{
			if q < p[i] + r[j-i] {
				q = max(q, p[i] + r[j - i]);
				s[j] = i;
			}
		}
		r[j] = q;
	}
	return r and s;
	////
	
--------------------------------------------------------------------------------------------------------------------------------------------------------
Matrix-chain multiplication:
----------------------------
We have a sequence of matrices A1, A2 ... An to be multiplied.
We can use the usual algorithm to multiply pairs of matrices then recall it to get the final product, for example:
If we have A1, A2, A3, A4, A5, then we can:
	(A1(A2(A3*A4)))
	(A1((A2*A3)A4))
	((A1*A2)(A3*A4))
	((A1(A2*A3))A4)
	(((A1*A2)A3)A4)
	
Normal algorithm:
-----------------
////
mulMat(A, B)
{
	if A.cols != B.cols {
		return error "Incompatible dimensions";
	}
	C = new matrix[A.rows, B.cols];
	for(i = 0 ; i < A.rows ; i++)
	{
		for(j = 0 ; j < B.cols; j++)
		{
			C[i][j] = 0;
			for(k = 0 ; k < A.cols ; k++)
			{
				C[i][j] += A[i][k]*B[k][j];
			}
		}
	}
	return C;
}
////

Time complexity: O(n**3) !!

Note that parenthesization can have a big difference.
	
The problem we have is:
Design an algorithm to determine the suitable parenthesization for a sequence of matrices such that we have the MINIMUM number of scalar multiplications.

Step 1: characterize the optimal solution structure:
	First, let Aij be the product of Ai, Ai+1, Ai+2 ...Aj
		Aij = Ai*Ai+1*...Aj
			= Aik * Ak+1j where i <= k <= j
	The cost of calculating Aij is the cost of calculating Aik + cost of calculating Ak+1j + cost of calculating both result matrices.
	
*** to be continued ***
------------------------------------------------------------------------------------------------------------------------------------
Elements of dynprog:
--------------------
So, when can we apply the method of dynprog to a problem?
Here we define some key ingredients that, if we find in a problem, we opt to dynprog.

A) Optimal substructure:
------------------------
It's a property in a problem which means that the optimal solution to the problem consists of / contains optimal solutions of subproblems.
In dynprog we build optimal solutions to problems using optimal solutions to subproblems.
It's like building a very strong wall using very strong bricks.

To discover optimal substructure property:
1- Show a solution of the problem that consists of making a choice e.g. cutting a rod initially, such that the choice leaves subproblems.
2- Given this choice, determine which subproblems ensue and how to characterize the resulting space of subproblems.

You may have different subproblems when making a choice, but still have optimal substructure, in that case you need to solve each one then combine the soltions.
Usually, the running time of DynProg depends on the product of:
	i. number of subproblems overall, and
	ii. how many choices we look for each subproblem.
	
You can use a subproblem graph to perform the same analysis.

DynProg uses Optimal Substructure in bottom-up i.e. it solves subproblems optimally then finds an optimal solution to the problem.

Some problems may appear to have optimal substructure, but they don't e.g. Optimal Longest Simple Path between two nodes in a graph.
Actually, no efficient dynProg solution for that problem has been found.

This problem is classified as NP-Complete, which is an advanced topic but in layman's terms it means we're unlikely to find a polynomial-time solution for that problem.

Subproblems should be independent, the solution for one subproblem should not affect the solution for another subproblem.
In the longest simple path problem, subproblems are NOT independent.

B) Overlapping subproblems:
---------------------------
Now first to distinguish between some stuff:
- Two Independent subproblems: two subproblems where the solution of the first one doesn't affect the solution of the second one and VV.
- Two Overlapping subproblems: two subproblems where to solve the first one we'll solve subsubproblems which are ALSO NEEDED to solve the second one.

So, two subproblems can be independent AND overlapping, they don't contradict each other.

When a recursive algorithm encounters the same subsubproblem many times when solving DIFFERENT subproblems, we say that these subproblems are overlapping.
The DnC algorithm may not encounter such thing, and greedy algorithms DEPEND on subproblems being NOT overlapping.

Basically, Dynamic programming is a DnC that takes advantage of that property, in a sense it doesn't trim the recursion tree but it doesn't go through all of it by memorizing values to subproblems so that it doesn't go down all the paths of the tree.

So, an algorithm for thinking:
	Can problem be divided into similar subpoblems?
	If yes:
		Do subproblems overlap?
		If yes:
			Dynamic Programming.
		If no:
			Do subproblems have greedy property?
				If yes:
					Greedy.
				If no:
					Divide & Conquer.
	If no:
		¯\_(ツ)_/¯
		
Memorization:
	Make a DnC algorithm, then modify it to memorize stuff, then recall that stuff.
	
Bottom-up approach has the advantage over memorization by constant factors if we need to solve subproblems only once.
Memorization has the leg up if there are subproblems that don't need to be solved.

---------------------------------------------------------------------------------------------------------------------------------------------------------------
Longest Common Subsequence:
---------------------------
S1 = ACCGGTCGAGTGCGCGGAAGCCGGCCGAA
S2 = GTCGTTCGGAATGCCGTTGCTCTGTAAA

We can say that two DNAs are similar if:
	- one is a substring of another, or 
	- the number of changes done on one to turn it into the other is small, or 
	- there's a common pattern between them, or 
	- there's a common pattern  but that pattern is not necessarily consecutive, just the order is preserved.

In our example the longest such pattern is:
S3 = GTCGTCGGAAGCCGGCCGAA

That last thing is called the longest subsequence, given two strings we want to find that madafaka.
A subsequence is the same sequence but with zero or more elements removed.

A sequence X = {x1, x2 ...] has a subsequence Z = {z1, z2, ... zk} if we have a strictly increasing sequence {i1, i2, ... ik} of indices in X such that for j = 1, 2, ... k we have x_i_j = z_j

So we can have two sequences X and Y, and Z is a subsequence of both i.e. a Common subsequence.

Our problem is to find the longest common subsequence between two sequences, or the LCS problem.

Step 1: characterize an LCS:
	First, the problem has optimal substructure property, as each character we detect as common, we're left out with another sequence that we have to do the same for. The optimal solution for each subproblem is to pick the longest common subsequence of that subproblem, which combined with other subproblems form the LCS of the problem.
	
	Theorem:
	--------
	Let X and Y, and let Z be an LCS:
	if xm = yn then zk = xm = ym and Z[k-1] is the LCS of X[m-1] Y[n-1] where these indices mean PREFIXES.
	else:
		zk != xm implies that Z is an LCS of X[m-1] and Y.
		zk != yn implies that Z is an LCS of X and Y[n-1].
		
	*** Proof ***
	
	Implication: if we have an LCS of two sequences, it contains an LCS of PREFIXES of the two sequences, proving the optimal substructure prop.
	
Step 2: a recursive solution:
	If xm = yn then we should find an LCS to X[m-1] and Y[n-1], and so on for the three cases of the theorem.
	i.e. it's a recursive solution.
	In both case 2 and 3, our solution to the resulting subproblems both contain the solution for case 1 i.e. overlapping subproblems, and that's recursive.
	Therefore, dynamic programming.
	
	So, we want to establish a recurrence for the value of the optimal solution.
	*** recurrence defined in page 414 ***
	
	