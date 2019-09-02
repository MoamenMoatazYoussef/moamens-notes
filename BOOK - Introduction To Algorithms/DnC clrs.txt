DnC CLRS: 
---------
Divide: the problem into subproblems.
Conquer: each subproblem independently.
Combine: the solutions to solve the whole problem.

Recursive by nature, so we have to define a base case (stopping condition).

Recurrence is key to DnC.

For example, mergeSort:
	T(n) = 2T(n/2) + theta(n)
		 = 2(2T(n/4) + theta(n/2)) + theta(n)
		 = 2(2(2T(n/8) + theta(n/4)) + theta(n/2)) + theta(n)
		 ...
		 ...
		 And so on.
		 
	This is equivalent to theta(n log n)
	
Divide doesn't have to be even, it can be 2:3 or 1:3 or whatever.

If division and combination take linear time, usually recurrence takes a form similar to (e.g. of 2/3 and 1/3 split):
	T(n) = T(2n/3) + T(n/3) + theta(n)
	
Solving a recurrence is figuring out the equation, and from the equation we can deduce the time complexity.

Three methods:
	- Substitution.
	- Recursion-tree.
	- Master method.
	
Also, recurrence doesn't have to be an equation, it can be an equality, these state an upper bound or lower bound of the algorithm.

We can ignore some technical stuff in recurrances, e.g. MergeSort's T(n) isn't the same if number of elements is odd. These things affect T(n) constantly, so the growth rate doesn't really change.

Also, we'll solve the recurrence without considering approximations and boundaries, then try to fit them.

Maximum subArray:
-----------------
Example use case: 
	- you want to buy a stock then sell it, you have the information about future prices of the stock, and you want to get the max profit i.e. buy lowest, sell highest.
	- Of course, the selling date should be after the purchase date.

Transformation into our problem:
	- We can design a brute-force algorithm to search for the appropriate dates, but it will be O(n^2).
	- What if instead, we consider another metric: the daily price CHANGE.
	- That way, we want to get a period of days where the sum of daily price changes is maximum i.e. maximum subarray problem ;)

Problem:
	- We have an array, and we want to find two indices i and j, where the sum of elements in the array from index i to index j is the maximum sub you can get in the big array i.e. the sum of this particular subarray [i, j] is bigger than any other subarray that can be generated from the main array. 

Analysis:
	- We have three cases:
		1- Price change is always positive, therefore the solution is the entire array.
		2- Price change is always negative, therefore the solution is the minimum negative number i.e. the max number in the array.
		3- Price change contains both positive and negative values: that's where we want our algorithm.
		
	- Array A[low, high] is our array,
		If we divide it in two: A[low, mid], A[mid + 1, high]
		Then either:
			i and j are in low array.
			i and j are in high array.
			i is in low and j is in high. In that case the array A[i, mid] actually should be the max subarray of A[low, mid], the same for the high part. Here's our subproblem.
			
		In all three cases, the subarrays are smaller instances of our problem, so we can use the DnC paradigm.
		
	- Also, we'll eventually arrive at a subarray where the max part HAS to cross the midpoint, in which case we find two smaller max subarrays and combine them.
	
Algorithm:
////
getMaxCrossingSubarray(A, low, mid, high) 
{
	maxLeft  = 0;
	maxRight = 0;
	
	leftSum  = -INF;
	rightSum = -INF;
	
	sum		 = 0;
	
	for i from mid DOWN to low: //Find first part
		sum += A[i];			//since we cross the midpoint, the subarray has to contain A[mid]
		if sum > leftSum:
			leftSum = sum;
			maxLeft = i;
		
	sum		 = 0;
	for j from mid + 1 UP to high: //Find second part
		sum += A[j];
		if sum > rightSum:
			rightSum = sum;
			maxRight = j;
	
	return (maxLeft, maxRight, leftSum + rightSum); //return indices for combined parts
}
////

Time complexity of this function: O(n)

Now, to find the max subarray anyway:
Algorithm:
////
maxSubArray(A, low, high)
{
	if(high == low)
		return A[low] //stopping condition
	
	mid = (low + high) / 2;
	
	leftLow, leftHigh, leftSum    = maxSubArray(A, low, mid);
	rightLow, rightHigh, rightSum = maxSubArray(A, mid+1, high);
	crossLow, crossHigh, crossSum = getMaxCrossingSubarray(A, low, mid, high);
	
	if leftSum > rightSum and leftSum > crossSum
		return leftLow, leftHigh, leftSum;

	else if rightSum > leftSum and rightSum > crossSum
		return rightLow, rightHigh, rightSum;

	else 
		return crossLow, crossHigh, crossSum;
}
////
How does that work?
	- First, array is divided into two by mid.
	- Then, we find the max subarray in the left part, if base case then will return one element.
	- Then same for right part, if base case then will return one element.
	- Then cross, if base case then it will be one of the elements or both.
	- Then selects the greater sum and returns the index of the subarray.
	
	- Then the same happens for every return of a call, until we reach all the way back where we'll have two arrays in left and right, then the cross will be found, then compared, and the max subarray will be returned.

Time complexity:
	- T(n) for the whole problem.
	- We divide it into two parts, each is a smaller instance, so T(n/2)
	- And we have a separate part with O(n), so theta(n)
	- Therefore:
	
	T(n) = T(n/2) + T(n/2) + theta(n)
	T(n) = 2T(n/2) + theta(n)
	
	This recurrence has the solutin O(n log n).

Analysis of solution:
	- As you can see, we needed to develop a recursive part for subproblems that are smaller instances of the main one, and a separate solution for the subproblem that isn't an instance of the main one.
	- O(n log n) much better than O(n^2).
	
------------------------------------------------------------------------------------------------------------------------------------
Strassen's algorithm for matrix multiplication:
-----------------------------------------------
First of all, matrix multiplication is normally done by multiplying a row by a column, to get one element.
Usually this means we'll loop over a row, a column, to get one element.

Getting one element means looping over the whole row and the whole column then add all of these. O(n)

So, to get one column, we loop over all rows and one column. O(n*n)
To get all rows, we loop over all column and one row. O(n*n)

To get all columns and rows, we loop over all columns and all rows. O(n*n*n) = O(n^3)
Which is horrible.

Strassen's algorithm improves the complexity a little bit: O(n^2.81)
--------------------------------------------------------------------	
A * B = C
We assume each to be of size n*n, and n is a power of 2.

If we divide A into four matrices like this:
A = [A11 A12; A21 A22];

Where each of A11, A12, A21, A22 are of size (n/2)*(n/2)

Same for B and C.

Then:
	C11 = A11*B11 + A12*b12
	C12 = A11*B21 + A12*b22
	C21 = A21*B11 + A22*b12
	C22 = A21*B12 + A22*b22
	
Each eqn is two muls of matrices of size n/2 and addition of them.

We can use this to create a DnC algorithm:

Algorithm:
////
matMul(A, B)
{
	n = A.rows;
	C = new matrix(n, n);
	if n == 1: //Stopping condition
		c11 = a11 * b11;
	else:
		PARTITION MATRICES;
		C11 = matMul(A11, B11) + matMul(A12*b12);
		C12 = matMul(A11, B21) + matMul(A12*b22);
		C21 = matMul(A21, B11) + matMul(A22*b12);
		C22 = matMul(A21, B12) + matMul(A22*b22);
	return c;
}
////

How does this work?
	Well, instead of multiplying we break the thing into smaller and smaller matrices until we reach size = 1, then we simply do the multiplication with no addition since the matrices are of 1 so each row and col is actually one element.
	
	Then, we return the result and do the same for the other side, then add the two sides and go up a level, then repeat, until we reach the top.
	
	Here, we traverse the recursive tree in a way that's really similar to DFS.
	
	
	The step PARTITION MATRICES is very important, if we copy entries to new matrices it would take O(n**2), so no improvement.
	
	But, if we do INDEX calculations, we can partition the matrices in O(1) time!!

Time complexity:
	First, the first part is of O(1) -> O(1)
	Then, second part we partition matrices in O(1) -> O(1)
	Then, we make a recursive call for two n/2 matrices T(n/2), but this is done 8 times -> 8T(n/2)
	Then, the resulting matrices are of size n/2 and contain (n**2)/4 elements, adding them is O(n**2) -> O(n**2)
	We also use index calculations to put the results in their place -> O(1) per entry
	
	T(n) = 8*T(n/2) + O(n**2)
	
	From the master theorem (later), the solution is O(n**) which is the same as the straightforward method.
	
	The trick comes when we count the constant factors that are ignored by the asymptotic notation:
		O(1) in partitioning depends on the number if matrices, so it's actually O(k).
		O(n**2) is actually O(n**2 / 4) because we add PARTITIONED matrices.
		
		But we can't just ignore the 8 calls for T(n/2), in the recursion tree of 2T(n/2) we have two children per node, but this time we have 8 children so we can't ignore it!
		
		Usually recursive notation of T(n/2) does NOT ignore constant factors.
		
		So, the key is to reduce the nodes in the recursion tree a little bit.
		
Strassen's method:
------------------
Idea: to trade one matrix multiplication from the 8, for many matrix additions in constant time.
Steps:
1- Divide matrices into n/2 -> O(1)
2- Create 10 matrices S1, S2...S10, each is n/2 and each is the sum of difference of two matrices created in step 1 -> O(n**2)
	S1  = B12 - B22
	S2  = A11 + A12
	S4  = A21 + A22
	S3  = B21 - B11
	S5  = A11 + A22
	S6  = B11 + B22
	S7  = A12 - A22
	S8  = B21 + B22
	S9  = A11 - A21
	S10 = B11 + B12

3- Using matrices created in 1 and 2, compute 7 products P1, P2...P7, each n/2;
	P1 = A11 * S1 = A11*B12 - A11*B22
	P2 = S2 * B22 = A11*B22 + A12*B22
	P3 = S3 * B11 = A21*B11 + A22*B11
	P4 = A22 * S4 = A22*B21 - A22*B11
	P5 = S5 * S6  = A11*B11 + A11*B22 + A22*B11 + A22*B22
	P6 = S7 * S8  = A12*B21 + A12*B22 - A22*B21 - A22*B22
	P7 = S9 * S10 = A11*B11 + A11*B12 - A21*B11 - A21*B12
	
4- Get C11, C12, C21, C22 by adding and subtracting combinations of P matrices -> O(n**2)
	C11 = P5 + P4 - P2 + P6 = A11*B11 + A12*B21 -> Q.E.D.
	C12 = P1 + P2 			= A21*B11 + A22*B21 -> Q.E.D.
	C21 = P3 + P4 			= A21*B11 + A22*B12 -> Q.E.D.
	C22 = P5 + P1 - P3 - P7 = A22*B22 + A21*B12 -> Q.E.D.
	
As we can see, we've simply traded one multiplication for many additions in constant time.

Time complexity:
	T(n) = 7*T(n/2) + O(n**2)
	
	Using the master theorem, complexity = O(n**(1g7)) ~ O(n**2.81)
	
---------------------------------------------------------------------------------------------------------------------------------------------------------
Solving recurrences: Substitution method:
-----------------------------------------
Idea: we now know how to derive the recurrence of a DnC algorithm, now we want to solve it to obtain the complexity.

Steps:
1- Guess the form of the solution.
2- Use math induction to prove your guess correct.

We use this method to establish upper and lower bounds for a recurrence, e.g. this recurrence:

T(n) = 2T([n/2]) + n

We guess that:

T(n) = O(n log n)

Since it's similar to previous recurrences, now we want to prove that to be an upper bound i.e. 

T(n) <= cn log n

Where c is a constant we choose.

Proof:
	- First we assume that this bound holds for all positive values m < n
		In particular m = [n/2]
		Substituting:
			T([n/2]) <= c*[n/2] log [n/2]
		
		Then substituting in the recurrence:
			T(n) <= 2(c*[n/2] log [n/2]) + n
				 <= cn log [n/2] + n
				 <= cn log n - cn log 2 + n
				 <= cn log n - cn + n
		
		From that we can see that we've reached the proof even if some terms lie around.
		Therefore:
			T(n) <= cn log n   
				Q.E.D.
				
		So, T(n) = O(n log n) as long as c = 1;
		
	- Now we want to prove that this holds for boundary conditions as well, the boundary can be derived from the base case of the recursion.
		For exmaple if the base case has T(1) = 1
		
		T(1) <= c*1*log1 = 0 ----> Contradicts our boundary condition that T(1) = 1

		So now, we want to find another base case that serves as the boundary of the proof i.e. for n > n0 where n0 is a constant that WE CHOOSE.
		Note that this is different from the base case of the recursion.
		
		For example, in our relation, the recurrence doesn't depend on T(1) for n > 3
		So we can replace it with T(2) for example, where n0 = 2.
		
		Now, we get T(2) and T(3) from T(1) with the recurrence.
		
		So now we can complete our proof: prove that T(n) <= cn log n where c >= 1 is large enough so that T(2) and T(3) hold too.
		
		T(2) = 4 <= c*2 log 2
		T(3) = 5 <= c*3 log 3
		
		This holds if c >= 2.
		
So how do we generate a good guess?
	There's no straightforward way, but there are some heuristics:
		T(n) = any form containing T(n/2) usually converges to some form of O(n log n)
		
	Subtleties: sometimes you guess something correctly but mathematically it doesn't hold, usually some subtraction of lower-order terms works.
	e.g.:
		T(n) = 2T(n/2) + 1
		
	If we try to guess that T(n) = O(n) then we should prove that:
		T(n) <= cn
		
	Which won't work, we'll reach a result:
		T(n) <= cn + 1
		
	So, we can subtract a lower order term from our previous guess:
		T(n) <= cn - d
		
	So now working the proof:
		T(n) <= c(n/2 - d) + (n/2 - d) + 1
			 <= cn - 2d + 1
	Therefore:
		T(n) <= cn - 2d
		
	Which can be right if we choose an appropriate c.
	
*** Avoiding pitfalls ***
	