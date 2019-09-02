Quicksort:
----------
Worst-case time complexity: O(n**2)
But,
It's usually the best practical choice for sorting, why?
	On average, it's REALLY efficient ^_^
	
Average case: O(n log n) and the constant factors are really small.
in-place, and works well even in virtual-memory environments.

Going deep in quicksort:
------------------------
It's a DnC algorithm:
	Divide: partition the array A[p..r] into two subarrays A[p..q-1] and A[q+1..r] where each element in A[p..q-1] is smaller than A[q], which in turn less than or equal to elements in the other half.
	
	Conquer: sort the two subarrays by recursive calls to quicksort.
	
	Combine: actually, no work is needed to combine as now both arrays are sorted in-place.
	
Algorithm:
////
sort(A)
{
	Quicksort(A, 0, A.length - 1);
}

Quicksort(A, p, r)
{
	if p < r
	{
		q = Partition(A, p, r);
		Quicksort(A, p, q-1);
		Quicksort(A, q+1, r);
	}
}

Partition(A, p, r)
{
	x = A[r]; //last element, or PIVOT
	i = p - 1; //index just before start
	for j = p to r - 1 //from start to just before end, i will trail behind j I think.
	{
		if(A[j] <= x) 	//if element is smaller than or equal to pivot, increment i and swap the element at j with the element at i 
			i++;		//(keeping smaller elements behind the middle point), until you reach pivot, not elements from p to q-1 are less than pivot.
			swap(A[i], A[j]);	//And elements from q are greater than pivot
	}
	swap(A[i + 1], A[r]); //finally swap pivot with element at q
	return i + 1;
}
////

*** loop invariant part ***

Performance of quicksort:
-------------------------
Partitioning can be balanced or unbalanced.
If balanced, the algorithm runs as efficiently as mergesort.
If unbalanced, it can run as slow as insertion sort.

Worst-case: That happens if partitioning produces a subarray with n-1 elements and a subarray with 0 elements.
	*** proof of the worst case ***
	
	That will cost O(n) and the 0 subarray will cost O(1).
	If that happens in each partitioning call:
		T(n) = T(n - 1) + T(8) + theta(n)
			= T(n - 1) + theta(n)
			
	If we sum the costs incurred at each level of the recursion, we'll get an arithmetic series which will evaluate to theta(n**2) :(
	
	This occurs a lot if the array is already completely sorted (Compared to insertion sort which takes O(n) time if the array is sorted).

Best-case: the most even possible split.
	T(n) = 2T(n/2) + theta(n)
		 => O(n log n)
		 
Average-case: Balanced partitioning, but how?
	With the same proportional split at every recursion level.
	e.g. 
		T(n) = T(9n10) + T(n/10) + theta(n)
	
	If we construct a recursion tree, in each level it costs cn, until we reach the boundary at depth log(0) n = theta(log n)
	And with theta(n)
		T(n) = O(n log n)
		
	So, balanced doesn't mean 1-to-1, maybe 9-to-1, maybe even 99-to-1, the key is that as long as this ratio is CONSTANT, the depth of the tree is ultimately O(log n), and the cost between levels is cn, so it will still be O(n log n).
	
Usually though, this doesn't happen, partitioning produces some balanced and some unbalanced, distributed randomly in the recursion tree, but usually bad splits result in O(n - 1) which is "absorbed" in the O(n) of the good splits, so the running time of a random quicksort usually is close to the running time of the best case, but with slightly larger constants hidden in the O(n).

