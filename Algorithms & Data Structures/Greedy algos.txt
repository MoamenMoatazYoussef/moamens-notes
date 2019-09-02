Greedy algos:
-------------
Format:
	Algorithm name
	Concept
	Why
	How
	Algorithm pseudocode
	When to use
	When not to use
	Example implementation
	Example problems
-----------------------------------------------------------------------------------------------------------------
Greedy algorithm main idea:
---------------------------
Concept:
--------
You're playing chess, and it's your move.
Sometimes you attack, sometimes you defend, sometimes you advance, sometimes you fortify, sometimes you sacrifice pieces so you get an advantage, etc.
And based on which strategy you'll follow, you choose your next move.

Now, if you play with a constant strategy that is "Choose the move that gives me the  biggest immediate advantage", this isn't a guaranteed approach as long-term consequences can put you at a disadvantage, but it's a good strategy nonetheless.

This is called the greedy strategy, you look for the move that will get you the biggest immediate advantage.
In the greedy algorithm we do just that, we think of the move that will give us the biggest immediate advantage.

So, usually the input for the greedy algorithm consists of many items and we should choose or sort or etc.
And to make a choice we define our benefit, and define the metric that upon which we measure the benefit we get from each next move, then choose the biggest one.

If it includes sorting, it's complexity will be around O(n*log n).

Why?
----
Used for optimization problems: maximization, minimization, etc.

Greedy aims to take optimal steps in order to get an optimal solution, so usually if a problem can be solved by greedy then this is the best approach.
It's more efficient than DynProg but it can't be applied to everything.

Example problems:
-----------------
1. Kruskal's minimum spanning tree:
	https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/
	
	Spanning tree: in a graph, a tree that connects all vertices of a graph together without cycles is a spanning tree.
	MST: a spanning tree that has minimum total weight.
	
	If a graph has V vertices, the MST has V-1 edges.
	
	Kruskal's greedy algorithm: Given V vertices:
	---------------------------------------------
		1. Sort all edges in the graph in descending by weight. (Metric of benefit)
		2. Pick smallest edge. (Maximizing the benefit)
		3. Check if smallest edge causes cycle. (Use the Union find algorithm)
			Causes cycle? discard and go back to step 2.
			Doesn't? include it in the tree.
		4. Repeat step 2 until you have V-1 edges.
		
	As you can see, we don't need to start and end at certain vertices, edges can be disconnected while we run the algorithm and get connected gradually until we fully construct the MST.
	
	Complexity:
	-----------
		Sorting: O(E*logE) //E = number of edges.
		Find-union to check for cycles: O(logV)
		Checking for cycles for all edges: O(E*logV)
			Total: O(E*logE + E*logV)
		The value of E can be almost O(V^2), so O(logE) ~ O(logV)
			So total can be considered: O(ElogE) or O(ElogV)

	
2. Prim's minimum spanning tree.

3. Dijkstra's shortest path algorithm.