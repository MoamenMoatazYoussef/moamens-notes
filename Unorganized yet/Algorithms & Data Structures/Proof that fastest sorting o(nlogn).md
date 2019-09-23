Best time complexity of any Comparison-based sorting algorithm:
---------------------------------------------------------------
Any sorting problem:
- ip: sequence of numbers a1, a2 ...an
- op: permutation of input such that a1 <= a2 <= a3 ... <= an.

A comparison-based sorting algorithm uses comparison between two elements to sort them.

We can represent them in decision trees: a decision tree is a full binary tree (A tree where each node has 0 or 2 children) representing  comparisons between elements.

Executing the algorithms meaning tracing a path from the root to a leaf, where in each node along the way a comparison (ai <= aj?) is made:
- true: we go to the left subtree.
- false: we go to the right subtree.

When we reach a leaf, the sorting algorithm has established the ordering.

We have n elements so we have n! permutations, and each possible permutation is achieved by an order that is dictated when the algorithm traverses from the root to a certain leaf.

So each permutation should appear as a leaf.

If the max number of comparisons of a sorting algorithm is X:
	Max height of tree = X.
	Max leaves in tree of height X = 2^X.
	
Math:
-----
	n! <= 2^x //since n! is permutations and 2^x is possible number of leaves.
	
therefore: 
	log2(n!) <= x
	
since:
	log2(n!) = O(n log n) asymptotically
	
therefore:
	x = O(nlog n)
	
The fastest sorting algorithm which sorts n elements by making at most x comparisons, will have time complexity O(n log n).

MergeSort and HeapSort are optimal algorithms.