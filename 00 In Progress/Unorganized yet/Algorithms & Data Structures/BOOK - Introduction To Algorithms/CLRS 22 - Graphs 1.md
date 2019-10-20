Graphs:
-------
A) Elementary graph algorithms:
-------------------------------
There are two standard ways to represent a graph:
1- Adjacency list.
: We have an array of lists where the head is a node in the graph, and the rest of the list is the nodes connected to the main node (kinda like the hash table).

2- Adjacency matrix:  

G = (V, E)
V: number of vertices.
E: number of edges.

The first way is good because it provides a compact way to represent SPARSE graphs where:
	E << V**2
The second way is good for dense graphs, where E is very close to V**2
The idea being if we used an adjacency matric with a sparse graphs there will be a lot of zeros in it, so the first way is better since it's more compact.
Both representations can account for weights, directed graphs, etc.
First is better in memory since it takes O(V + E), but slower in speed.
Second is faster, but takes more memory O(v**2). (Independent of E, which is why it's preferred only in dense graphs).
If the graph is undirected, we'll find that the adjacency matrix will be equal to its transpose.
So sometimes we only store the entries on and above the diagonal of the matrix, saving half of the memory.

----------------------------------------------------------------------------------------------------------------------------
Breadth-first search (BFS)
--------------------------
Prim's and Dijkstra's are based on this.
G = (V, E), given a source vertex s, BFS explores edges of G to discover reachable vertices from s, then does the same for all the reached vertices, and so on.
Works on directed and undirected.
Undiscovered vertices are white, 
discovered vertices are gray, 
discovered vertices with adjacent vertices also discovered are black.

So BFS starts on gray, discoveres all adjacent white then colors source black, then goes to each adjacent vertex and colors it black, once all adjacent vertices are black it goes to the next level of gray stuff, etc.

BFS constructs a BF tree, containing s as root, then adjacent nodes, etc.
When a new vertex v is discovered from u the edge uv is added to the tree.
U is the predecessor or parent of v in the tree.

Algorithm:
----------
Assuming graph is represented using lists, we use three new attributes:
u.color = {w, g, b}
u.pi = predecessor of this node
u.d = distance from s to v.
We also use a queue Q.

////
bfs(G, s)
{
	for each vertex V - {s}:
		u.color = w;
		u.pi	= NULL;
		u.d		= INF.
	s.color = g;
	s.d 	= 0;
	s.pi	= NIL;
	
	Q = {};
	enqueue(Q, s);
	while(Q != {}):
		u = dequeue(Q);
		for each vertex v adjacent to u (from the adjacency list):
			if v.color = white:
				v.color = g;
				v.d		= u.d + 1;
				v.pi	= u;
			enqueue(q, v);
	u.color = b;
}
////

Analysis:
---------
T(n) = O(1) + O(v) + f(E)

Because we scan the adjacency list of a vertex only when it's dequeued, we do this only once per vertex.

T(n) = O(V + E) #

*** Maths section ***

----------------------------------------------------------------------------------------------------------------------------
Depth-first search (DFS):
-------------------------
Explore all edges of the most recently discovered vertex v1, then explore most recent vertex discovered from v1, and so on until no new stuff, then backtrack one step and repeat.
DFS constructs many DF trees, or a DF forest.

Same coloring technique, this ensures that nodes end up in ONE of the trees, so the trees are disjoint.

Also, two new attributes:
v.discover = time when grayed.
v.finished = time when blacked.
this time is expressed in integers starting from 0, not actual time.
These help provide some info about the graph.

Algorithm:
----------
////
dfs(G)
{
	for each vertex u:
		u.color = w;
		u.pi	= NULL;
	time = 0;
	for each vertex u:
		if u.color = w:
			dfs-visit(G, u)
}

dfs-visit(G, u)
{
	time++;
	u.discovered = time;
	u.color = g;
	for each vertex v adj[u]:
		if v.color = w:
			v.pi = u;
			dfs-visit(G, v)
	u.color = black;
	time++;
	u.finished = time;
}
////

Analysis:
---------
First loop runs O(V)
For each we discover edges once so O(E)

T(n) = O(V + E)
