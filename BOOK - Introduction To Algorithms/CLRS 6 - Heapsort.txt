Heapsort:
---------
Time complexity: O(n log n)
In-place sort i.e. #elements stored outside the array = constant and O(1)

Heap:
-----
It's an array that can be viewed as a nearly complete binary tree, the only level not complete may be the lowest one.
If we have an array, part of it may be a heap and the rest isn't i.e. we can have array A such that:
	A = [1, .., A.length]
But we can make a heap up to:
		[1, .., A.heap-size]
Where:
	0 <= A.heap-size <= A.length
	
The root of the tree is the first element A[0], for any node i if we have the index of the element it's easy to get the indices of its children and its parent.

////
Parent(int i)
{
	return i/2;
} //This is usually implemented by SHIFTING the binary representation of i by ONE to the right, instead of division, so it's efficient.

Left(int i)
{
	return 2*i;
} //This is usually implemented by SHIFTING the binary representation of i by ONE to the left, instead of multiplying, so it's efficient.

Right(int i)
{
	return 2*i + 1;
} //This is usually implemented by SHIFTING the binary representation of i by ONE to the left and then adding 1, so it's efficient.
////

Usually these are implemented as macros or inline procedures.

Types of heap:
--------------
Max heap: The value of any node is at most the value of its parent. (root is max element)
Min heap: The value of any node is at least the value of its parent. (root is min element)

Max heaps will be used for heapsort.
Min heaps are used for priority queues.

Height of a node: number of edges on the longest downward path from the node to a leaf.
Height of a heap: height if its root.

If the array is of size n (or the heap-size is = n), then the height of the heap is log n.
Operations on heap at most depend on the height, so they're O(log n).

Operations:
-----------
Max-heapify: a procedure that maintains the max-heap property.
Build-max-heap: runs in linear time and produces a max heap from array.
Heapsort: runs in O(n log n) and sorts the array in place.
Max-heap-insert, heap-extract-max, heap-increase-key, heap-max: These allow the heap to implement a priority queue.

Maintaining the heap property:
------------------------------
Max-heapify is a function that takes A and index i as args.
It assumes that Left(i) and Right(i) are max-heaps, but A(i) might be smaller than its children, violating the max-heap property.

Max-heapify lets A[i] float down to go to its place in the heap.

////
maxHeapify(A, i)
{
	l = Left(i);
	r = Right(i);
	if i <= A.heapsize && A[l] > A[i]
	{
		largest = l;
	}
	else
	{
		largest = i;
	}
	
	if r <= A.heapsize && A[r] > largest
	{
		largest = r;
	}
	
	if largest != i
	{
		exchange A[i] with A[largest];
		maxHeapify(A, largest); // to continue heapifying down the tree
	}
}
////

Time complexity:
	On one node it's O(1).
	A subtree has a size at most 2n/3, and we have two subtrees.
	Worst case when lowest level is exactly half-full.
	So:
		T(n) <= T(2n/3) + theta(1)
		
	Using the master theorem, this is case 2:
		T(n) = O(log n)
		
Building a heap:
----------------
We can use maxHeapify to build a heap out of an array, if we have A[1...n], then all elements from A[n/2 + 1] to A[n] are leaves, so they don't need heapification.

Instead, we start working from that index DOWN and heapify stuff.

////
buildMaxHeap(A)
{
	l 			= A.length;
	A.heap_size = l
	for(i = l; i >=0; i--)
	{
		maxHeapify(A, i);
	}
}
////

*** How does that work? ***

Time complexity:
	maxHeapify: T(n) = O(log n)
	called for n elements:
		T(n) = O(n log n)
	
	But that's not asymptotically correct...
	
	The time of maxHeapify to run varies with the height of the node in the tree, and the height of most nodes are small.
	If we have height = log n and at most n/2**(h + 1) nodes at height h.
	
	So, the time required by maxHeapify is O(h) at node of height h.
	
	So, the upper bound may be expressed as:
		sum[h = 0 -> log n] (n / (2**(h+1))) = sum[h = 0 -> log n] (h / 2**h)
		
	Using the same approximation we used in DnC in the recursion tree:
		O(n * sum[h = 0 -> log n]) (h / 2**h) = O(n * sum[h = 0 -> INF] (h / 2**h)
	  = O(n)
	  
	So, the procedure buildMaxHeap takes O(n), linear time.
	
All these concepts apply to minHeapify and buildMinHeap.

---------------------------------------------
Heapsort algorithm:
-------------------
The algorithm starts by using buildMaxHeap from the array,
then put A[1] in the tree as the correct final position by exchanging it with A[n] in the array,
then we discard node A[n] from the heap and decrement heap-size,
then we observe that the children are max-heaps, but the new root may violate the property,
so we use max heapify,
then repeat the process.

Algorithm:
////
heapsort(A)
{
	buildMaxHeap(A);
	for(i = A.length; i <= 2; i--)
	{
		exchange A[1] with A[i];
		A.heap-size--;
		maxHeapify(A, 1);
	}
}
////

---------------------------------------------
Priority queue:
---------------
Unfortunately usually a good implementation of Quicksort beats heapsort in practice :(
But the heap DS can be used in a lot of things, notably an efficient priority queue.

What the hell is a priority queue?
A DS that maintains S elements each with an associated value or "key", ordering them by that key.

Operations:
	Insert(S, x): inserts x into S, equivalent to S = S U {x}.
	Max(S): returns the element with the largest key.
	Extract-max(S): pops the max.
	Increase key(S, x, k): increases key of element x to a new value k, which is assumed to be >= x's key.
	
Uses:
	Max-priority queue: It's used in OS development where a max-priority queue keeps track of jobs to be performed and their relative priorities.
	The scheduler uses Extract-max to get the job with the highest priority to execute it.
	
	Min-priority queue: it's used as an event-driven simulator, as some events should happen in order of occurance.
	
Using heaps to implement priority queues:
	In job scheduling or event-driven simulation, elements of priority correspond to objects in each application, and we want to determine which app object corresponds to a given priority queue element.
	
	This can be achieved by storing a "handle" to the corresponding object in each element, like a pointer for example or an array index i.e. integer.
	
Implementing operations of priority queue using heap:
////
Extract-max(A)
{
	if A.heap-size < 1
		error "heap underflow";
	max  = A[1];
	A[1] = A[heap-size];
	heap-size--;
	maxHeapify(A, 1);
	return max;
}

//Time complexity: O(log n), rest of operations are constant O(1)

increase-key(A, i, key)
{
	if key < A[i]
		error "New key is smaller";
	A[i] = key;
	while i > 1 && A[parent(i)] < A[i]
	{
		exchange A[i] with A[parent(i)];
		i = parent(i);
	}	
}

//Time complexity: O(log n)

max-heap-insert(A, key)
{
	heap-size++;
	A[heap-size] = -INF; //first insert a heap with value - inf then call increase key to insert it into its place.
	increase-key(A, heap-size, key);
}

//Time complexity: O(log n)
////

So, a heap can support any priority queue operation in O(log n), which is AWESOME!

---------------------------------------------------------------------------------------------------------------------------------------
Quicksort:
----------