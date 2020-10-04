Print all permutations of a string:
-----------------------------------
The paradigm is backtracking.

We begin with swapping 1st character with 2nd, third, fourth, etc.

Then eliminating this character, we now have n-1 characters, we do the same thing, swap the 1st character in the subproblem (which will be the second character) with 2nd, third, etc.

Each swap of each character generates a permutation.

If we construct a recursion tree we'd have solutions all over it!
In order to keep the solutions only in the leafs we may permit swapping the character with itself, so at the level just above the last one we have the two LAST characters to swap and obviously we swap the first with itself and with the second, getting two leafs.

So the root node will generate n nodes, each with n-1 characters to swap so they'll generate n-1 nodes, which hae n-2 to swap so they'll generate n-2 nodes ... and so on until we have two characters, this will generate two nodes and each have only one character to swap which is an end state.

Which explains the O(n!) complexity.

Note that, this is the time needed so that we REACH the permutations. Printing the permutations require O(n) time.

If there are repeating characters, this algorithm will print the repeated permutations.

Algorithm:
----------
////
void permute(string s, int left, int right)
{
	if(l == r)
	{
		System.out.println(str);
		return;
	}
	
	for(int i = left; i <= right ; i++)
	{
		str = swap(str, left, i);
		permute(str, left + 1, right); //Recursion to reach other subproblems
		str = swap(str, left, i); //Backtracking
	}
}
////

Time complexity: O(n * n!)