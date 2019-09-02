16- Greedy algorithms:
----------------------
The main concept is: what is the BEST move at the moment?

They don't always get optimal solutions for all problems, but more often than not, they do.

A) Activity selection:
----------------------
Scenario: we want to schedule many activities that use one common resource (e.g. many lectures and courses that use ONE lecture hall).

Goal: schedule them so you get the maximum number of activities within the limits of the resource consumed.

Settings: each activity a[i] has a start time s[i] and finish time f[i].

Procedure:
- We want to select activities that are "compatible" i.e. their time frames don't overlap.
- We want the maximum number of compatible activities.
- It's best to assume that activities are sorted ascending according to finish time.

activ. : 1, 2, 3, 4,   5,  6 
start  : 1, 3, 0, 8,   2, 12
finish : 4, 5, 6, 12, 14, 16

Activities 3, 4, and 6 are compatible.

Now, if we have a large set of activities, we can get many subsets of activities that are compatible, the objective is to select a subset that has the most number of activities.

Maths:

- If we have the set S, we can denote the set of activities between a_i and a_j as S_ij.

- If there exists a maximum subset A_ij such that it contains the activity a_k, we can divide it into A_ik and S_kj where the first is the set of activities that finish BEFORE a_k, and the same for S_kj.

- Therefore, to get the activities that are compatible with a_k and within A_ij, we want to interset A_ij with both S_ij and S_jk so that:

A_ik = A_ij ^ S_ik
A_kj = A_ij ^ S_kj

- So, the solution is the set of activities BEFORE a_k, with the set of activities AFTER a_k, with a_k itself:

A_ij = A_ik U {a_k} U A_kj

Where the maximum size is
|A_ij| = |A_ik| + 1 + |A_kj|

- So, A_ik must be the set of max activities for S_ik, and the same for kj. i.e. the optimal solution is achieved by taking optimal solutions of smaller parts. (This is called "Optimal substructure")

- Optimal substructures are related to dynamic programming somehow?


*** Dynprog section 15.3 ***

If the size of A_ij is c_ij, then we can get the recurrance:

c_ij = c_ik + c_kj + 1

- Therefore, the solution size can be:

c_ij_soln = max{c_ik + c_kj + 1}

So how can we solve this by greedy?
-----------------------------------

Well, instead of the recurrance stuff, we could just think in a greedy way: What's the activity that if we include in our solution, would leave room for the maximum number of activities?

The metric of benefit here is: earliest finish time possible.

THAT is why we sorted (or assumed) the activities by their finish time ^_^

Then, we scan the activities for the first activity that starts AFTER the first one finishes.

- But remember we'v deduced that this problem has optimal substructure, if we choose a1 as the first activity, we need to do the greedy choice again  but for S_1j where it's the set of all activities that start AFTER the first one finishes, sounds familiar? ;)

- That tells us that if there's an optimal solution, it consists of a1 and all the greedy-chosen activities from S_1j.

But is that right?
------------------
If we have subproblem S_k, where the earliest finishing activity is a_m, therefore a_m is included in one of the solution sets of S_k.

If A_k one of these sets, and let a_j be the activity in that set with the earliest finishing time:

if a_m == a_j:
That's Q.E.D.
	
else:
There exists A_k_dash where:

A_k_dash = A_k - {a_j} U {a_m}

Since A_k was a solution, the activities in it don't overlap, and by default if a_m finishes before a_j then A_k_dash's activities also don't overlap.

So, the size of A_k_dash is the same as A_k since we removed and added one set, and then A_k_dash would be the solution, also containing a_m, which is Q.E.D.

-------------------------------
As we can see, greedy algorithms don't work bottom-up like DynProg (solve subproblem THEN choose), they are top-down (Choose THEN solve resulting subproblem).

Algorithm (Recursive):
----------------------
////
//s: start
//f: finish
//k: where subproblem begins
//n: number of activities
selectActivity (s[], f[], k, n) {
	m = k + 1;
	while m <= n && s[m] < f[k]
		m++; //iterates until 1st activity AFTER a_k
	if m <= n:
		return {a_m} U selectActivity(s, f, m, n);
	return {};
}
////

Algorithm (iterative):
----------------------
////
selectActivity(s[], f[])
{
	n = s.length
	A = {a_1}
	k = 1
	for m from 2 to n:
		if(s[m] >= f[k])
			A U= {a_m}
			k = m
	return A
}
////
--------------------------------------
Elements of greedy:
-------------------
Steps we've taken:
1- get optimal substructure.
2- develop recursive solution.
3- if we pick a greedy soln, only one subproblem remains.
4- is it safe to make that choice?
5- Repeat using recursion.
6- Convert it into iterative.

General steps:
1- If we make a greedy choice, will we have only one subproblem to solve?
2- Is there an optimal solution that occurs when we make the greedy choice?
3- Is that true for the subproblem too? and if yes, this means that the solution set is the set of greedy choices of each subproblem.


- Beneath every greedy algorithm, there's a more cumbersome dynamic programming solution.

To know if the problem can be solved by greedy we check two properties:

a) Greedy-choice prop:
	In dynprog, solutions of subproblems depend on solutions of other subproblems.
	In greedy, solutions to subproblems don't depend on our choices and the choices don't depend on the solutions.
	
	We can make greedy more efficient using sorting or a good data structure like Priority Queue. (Both can be used in activity selection).
	
b) Optimal substructure:
	If optimal solution to problem = set of optimal solutions to subproblems.

Greedy VS dynProg:
------------------
A) 0-1 knapsack problem:

A thief wants to fill his knapsack with the most value possible, we have n items having v values and w weights.

Max weight the thief can carry is W, so the sum of weights of items in the knapsack must be less than W.

So optimization problem: MAX value whose sum(w) < W.

B) Fractional knapsack: the same problem, but the thief can take parts of items.

Both problems have optimal substructure property.
If the total load in 0-1 problem is x, and we remove item j, then the left should be the most valuable load whose weight is x - w_j.
Same for fractional problem.

Now, the fractional problem can be solved by greedy, where the benefit metric is value/weight for each item, then we just pick parts with the greatest value/weight until we fill the knapsack.

That, including sorting by this metric, is O(n log n).

But the 0-1 can't be solved using the same strategy, because we have to pick the whole item or not pick it, so we might end up picking small items with big value (not realizing we've leaving big items with much BIGGER value for example), or many other cases that won't be the optimum solution.

So, if we decide to change our method, we simply want to know the solution of our subproblem BEFORE picking items, because they're overlapping, sounds familiar?

Yep, dynamic programming.
