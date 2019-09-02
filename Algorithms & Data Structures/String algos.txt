KMP algorithm:
--------------
http://jakeboxer.com/blog/2009/12/13/the-knuth-morris-pratt-algorithm-in-my-own-words/

Partial match table: we break the pattern into an array, the array has index and value like any other array, like this:

char:  | a | b | a | b | a | b | c | a |
index: | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 
value: | 0 | 0 | 1 | 2 | 3 | 4 | 0 | 1 |

If we want to check the 7th cell for example, we only need to check the first 7 chars in the pattern. This is the use for the index.

What about the values?
First we need some definitions:
	- Proper prefix: all chars in a string without at least 1 char from the end.
	- Proper suffix: el3aks, 1 char from the start.
	
Let's say we're interested in the third cell, this means we need only the 1st three chars "aba", the proper prefix of that is "a" and "ab",
the proper suffix for that is "a" and "ba".

Notice that there's a proper prefix which is also a suffix (ahaaa xD) which is "a". The length of that is 1.

What if we're interested in the 4th cell? well, "abab"
Proper prefixes: a, ab, aba
Proper suffixes: b, ab, bab

Common: ab, length: 2

What if 5th cell? ababa
Proper prefixes: a, ab, aba, abab
Proper suffixes: a, ba, aba, baba
Common: aba, length: 3

What if 7th cell? abababc
Proper prefixes: a, ab, aba, abab, ababa, ababab
Proper suffixes: c, bc, abc, babc, ababc, bababc
Common: none..., length: 0.

8th cell? abababca
Common: c, length: 1

See? these are the VALUES in the partial match table.

Now, how can we use the table to skip unnecessary old comparisons?
I AM THE TABLE!
I AM THE VIEW!
I AM THE TABLE!

Let's say we're matching abababca with bacbababaabcbab, we have the same table.

The first time we'll get a partial match is at the second "a". The length of the partial match is 1, if we go to index i-1 we'll find that the value is 0, so we don't skip anything.

Next, we get a partial match at the fifth "a", where "ababa" match.
Now, this is a partial match of length 5, we go to table[5-1] and find the value to be 3.

What is 4? 4 is the length of the longest prefix that also qualifies as a suffix of this partial match, since it's common we know these will match, so we can skip some of them.

The idea is that, the prefix aba and the suffix aba match, so it's like the pattern consists of subpatterns that overlap at the common characters.

This means, if we move the pattern by some offset, the same comparisons will occur, so we can skip them.

How do we determine the amount of skip? 
skip = L - table[L] - 1;

Another example:

abcdefab
char:  | a | b | c | d | e | f | a | b |
index: | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 
value: | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 |

Since there's not much overlap between the subpatterns of the pattern, the only skip happens when we're interested in the entire pattern.

So at best it will skip (8-2-1) = 5 characters.

This algorithm is very useful when the pattern you're matching may have a lot of subpatterns in them, for example binary data.

if (partial match of length L is found) and (table[L] > 1):
	skip = L - table[L] - 1;
------------------------------------------------------------------
Finite Automata Algorithm for pattern searching:
------------------------------------------------
We construct FA for the pattern.
Then use it to search,
Which makes searching more efficient.

Complexity: O(n)

Number of states = m+1 where m is length of pattern.

Construction of the FA takes O(m^3*c) c is number of chars possible in patter or text.
There are efficient ways to construct the FA.

////
	static int NO_OF_CHARS = 256; 

	//Function that transfers to next state from current state
	static int getNextState(char[] pat, int M, 
							int state, int x) 
	{ 
		
		// If the character c is same as next 
		// character in pattern,then simply 
		// increment state 
		if(state < M && x == pat[state]) 
			return state + 1; 
			
		// ns stores the result which is next state 
		int ns, i; 

		// ns finally contains the longest prefix 
		// which is also suffix in "pat[0..state-1]c" 

		// Start from the largest possible value 
		// and stop when you find a prefix which 
		// is also suffix 
		for (ns = state; ns > 0; ns--) 
		{ 
			if (pat[ns-1] == x) 
			{ 
				for (i = 0; i < ns-1; i++) 
					if (pat[i] != pat[state-ns+1+i]) 
						break; 
					if (i == ns-1) 
						return ns; 
			} 
		} 

			return 0; 
	} 

	/* This function builds the TF table which 
	represents Finite Automata for a given pattern */
	static void computeTF(char[] pat, int M, int TF[][]) 
	{ 
		int state, x; 
		for (state = 0; state <= M; ++state) 
			for (x = 0; x < NO_OF_CHARS; ++x) 
				TF[state][x] = getNextState(pat, M, state, x); 
	} 

	/* Prints all occurrences of pat in txt */
	static void search(char[] pat, char[] txt) 
	{ 
		int M = pat.length; 
		int N = txt.length; 

		int[][] TF = new int[M+1][NO_OF_CHARS]; 

		computeTF(pat, M, TF); //construct FA

		// Process txt over FA. 
		int i, state = 0; 
		for (i = 0; i < N; i++) 
		{ 
			state = TF[state][txt[i]]; 
			if (state == M) 
				System.out.println("Pattern found "
						+ "at index " + (i-M+1)); 
		} 
	} 

	// Driver code 
	public static void main(String[] args) 
	{ 
		char[] pat = "AABAACAADAABAAABAA".toCharArray(); 
		char[] txt = "AABA".toCharArray(); 
		search(txt,pat); 
	} 
} 

// This code is contributed by debjitdbb. 

----------------------------------------------
construction of FA efficiently in O(m*c) only:
----------------------------------------------
We can use the longest prefix suffix idea of the KMP algorithm to construct the FA:

https://cdncontribute.geeksforgeeks.org/wp-content/uploads/autometa2.png

We usually represent it as a table where if we are in state i and we find character c we go to state FA[i][c].

Algorithm:
	no_states 	= Get number of chars;
	c[]			= Get distinct chars;
	Make table[no_states][c.length](zeros);
	table[0][c[0]] = 1;
	Fill rest of 1st row with zeros;
	int lps = 0;
	for row(i) from 1 to M:
		Copy entries from row[lps]; 
		table[i][c[i]] = i+1; 		
		lps	= table[lps][c[i]];		
		
Complexity: O(m*no_of_possible_chars).

Also good for cases	where patterns have a lot of subpatterns.
-----------------------------------------------------------------------
Z algorithm:
------------
Complexity: O(n + m)

Given a string s, to get the Z array we do this:
	First the array's length is the same as s's length (similar to lps).
	
char:  a | b | a | c | a | b | a
index: 0 | 1 | 2 | 3 | 4 | 5 | 6
value: X | 0 | 1 | 0 | 3 | 0 | 1

We know the index. what is the value.
Well, consider all proper prefixes of S:
a, ab, aba, abac, abaca, abacab

Now, for example, starting from the 5th element (index 4), what are the substrings we can make starting from here?
a, ab, aba

Now, from the set of proper prefixes and the set of substrings starting from index 4, what are the common elements?
a, ab, aba

What is the longest common element?
aba

What is its length?
3
That's the value we put in the array ^_^
That element is called the longest substring that is also a prefix, which is very confusing but you get the idea.

Algorithm: construct the Z array for a given string:
----------------------------------------------------
Naive:
------
def constructZ(String s): //O(n^2) bad!
	int[] z = new int[s.length];
	z[0]    = 0; //doesn't matter this value since the entire string is obviously a prefix of itself
	
	int longest = 0;
	int i 		= 1;
	int j       = 0;
	
	for i from 1 to length of string:
		while s[i + j] == s[j]:
			longest++;
			j++;
		z[i] = longest;
enddef;
-------------------------
Efficient:
----------

-------------------------
How to use the Z array:
-----------------------
We concatenate p to t such that "P$T"
Example: p = aab, t = baabaa

P$T = "aab$baabaa"

Build the Z array for the concatenated string.

Z = {X, 1, 0, 0, 0, 3, 1, 0, 2, 1}

Now, the length of p is 3, and the z array contains the value 3, therefore the pattern is present at:
	 (i - pattern.length() - 1) 















