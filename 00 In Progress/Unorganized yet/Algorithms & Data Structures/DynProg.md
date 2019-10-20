Dynamic Programming:
--------------------
In recursion, if we see recursive solutions that have repeated calls of the same input, we can use the results of these calls.

We store results of subproblems so that we don't have to calculate them again.

In recursion, we usually construct a recursion tree and in that tree we'll find calculations that are repeated a lot.

Dynamic programming is storing the values of calculations so that instead of doing these repeated calculations we just fetch their results.

Reducing the complexity from Exponential to Polynomial :o

If a function calls TWO recursive calls, it's most likely O(2^n).

*** Fibonacci numbers ***

How to store values:
1. Tabulation.
2. Memorization.

Tabulation:
-----------
Bottom-up method, we accumulate answers from the bottom to the top.

For example, calculating factorial:

int dp[MAXN];

dp[0] = 1;

for(int i = 1; i <= n; i++)
{
	dp[i] = dp[i-1]*i;
}

Here we start from the BOTTOM and calculate up, we populate our table sequentially since we're accessing the calculated values from the table itself (i.e. the array)

Summary:
- Begin with table.
- Iterate filling values from bottom.
- Update next using prev solutions.
- Return table[n].

When to use it?
---------------
- Doesn't use recursion, so saves overhead of function call and checks.

Memorization:
-------------
Top-down approach, we start from the DESTINATIOn and go down.

For example, calculating factorial

int dp[MAXN]; //initialized to -1

int solve(int x)
{
	if(x == 0)
		return 1;
	if(dp[x] != -1)
		return dp[x];
	return(dp[x] = x * solve(x-1));
}

Summary:
- Begin with table.
- Init all values = NIL.
- Call function which is recursive.
- If table[i] != nil, get the value.
- Else, if satisfies condition:
	- Update table[i].
	- Else: split into subproblems and calculate value.
	
The memorization will take effect once we reach the satisfying solution i and calculate stuff.

We memorize the answers so that we can use them AFTER recursion finishes and we are RETURNING from functions.

When to use it?
---------------
- Doesn't usually calculate ALL subproblems.

Overlapping subproblems:
------------------------
DynProg is similar to DnC in its approach, it's needed when the solutions of subproblems are needed again and again.

So dynamic programming can't be used for merge sort or binary search for example.

To know this it's helpful to draw the recursion tree, any repeated calculations will show.

In memorization technique:
1. Initialize a lookup array.
2. Whenever you need a solution of a subproblem:
	- Look in the table.
	- If present, get it.
	- If not, calculate it.
	- Then store it in the table.
	
//Memorization (top down)
class Fibonacci
{
	public int[] table;
	public void initLookupTable()
	{
		for(int i = 0 ; i < MAX ; i++)
			table[i] = -1;
	}
	
	public int fibonacci(n)
	{
		if(n <= 1)
			table[n] = n;
		else
			table[n] = fibonacci[n-1] + fibonacci[n-2];
		return table[n];
	}
}

//Tabulation (bottom up)
class Fibonacci2
{
	public int[] table;
	public void initLookupTable()
	{
		table[0] = 0;
		table[1] = 1;
	}
	
	public int fibonacci(n)
	{
		for(int i = 2; i < MAX; i++)
			table[n] = table(n-1) + table(n-2);
	}
}

Both tabulation and memorization are subproblems.


WHEN to use dynprog:
--------------------
If subproblems are overlapping.

-------------------------------
Optimal substructure:
---------------------
If optimal solution can be given by optimal solutions of subproblems, this is an optimal substructure property.

e.g.:

The shortest path from node x to node y, provided that they can traverse through another node v, is the combination of shortest path from x to v and from v to y.






How to solve a DynProg problem?
-------------------------------
1. Identify DynProg problem.
	- Maximize.
	- Minimize.
	- Counting.
	- Satisfy overlapping subproblems.
	- May satisfy optimal struct property.
	
2. Decide a state and how to transition from it to another state.
	- State: a "position" in the problem, like fib(n-1).
	
	Aim for least no. of params in each state e.g. fib(n-1) only has n as a param.
	
3. Formulate a relation among states.
	- Come up with an equation or a relation between states that is consistent throughout states.
	
4. Add memorization or tabulation.