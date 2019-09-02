BSTs:
-----
A binary tree, consisting of nodes which have (data, *left, *right).

The BST is usually complete, so the height is log n.

In practice, trees are not usually guaranteed to be constructed randomly, so we opt for variations of BSTs e.g. red-black trees, B-trees (Which are useful for maintaining data on a hard disk).

The defining property is the BST property: at node i: 
	- left->key <= i.key.
	- right->key >= i.key.

We can print all elements in order by a simple recursive algorithm using the inorder traversal:
////
inorder(node *n)
{
	if(n != NULL)
	{
		inorder(n->left);
		print(n->key);
		inorder(n->right);
	}
}
//// 
Time complexity: 
----------------
Base case:
	T(0) = c 	
Just check condition and return, constant time.

If node n has left subtree containing k nodes, then the right subtree will have n - k - 1 nodes.

And for some constant d > 0:
	T(n) <= T(k)+T(n-k-1)+d	
		 <=((c+d)k+c)+((c+d)(n-k-1)+c)+d
		 <=(c+d)n + c
		 <= O(n)
		 
Therefore, runtime: O(n)
----------------------------------------
Operations:
-----------
- Search
- Min
- Max
- Predecessor
- Successor
- Insert
- Delete

All are proportional to the height of the tree, which is log n, so they take around O(log n), and O(n) at worst case.

Searching:
----------
//// Recursive
search(node* n, value v)
{
	if(n == NULL or n.key == v)
		return n;
	if(n.key >= v)
		return search(n->left, v);
	else
		return search(n->right, v);
}
////

//// Iterative
search(node* n, value v)
{
	while(n != NULL & v != n.key)
	{
		if(n.key >= v)
			n = n->left;
		else
			n = n->right;
	}
	return n;
}
////

Minimum and Maximum:
--------------------
By definition of the structure, the minimum element is the leftmost child of the tree, same for maximum it's the rightmost child of the tree:

////
min(node* n)
{
	while(n->left != NULL)
		n = n->left;
	return n->key;
}

max(node* n)
{
	while(n->right != NULL)
		n = n->right;
	return n->key;
}
////

Both have O(h) where h is height of the tree i.e. O(log n)

Successor and Predecessor:
--------------------------
Successor is the value that is just after the value in the node n, predecessor is the one just before it.

////
successor(node* n)
{
	if(n->right != NULL)
		return min(n->right);
	let p = n.parent;
	while(p != NULL and n == p->right)
	{
		n = p;
		p = p.parent;
	}
	return p;
}

predecessor(node* n)
{
	if(n->left != NULL)
		return max(n->left);
	let p = n.parent;
	while(p != NULL and n == p->left)
	{
		n = p;
		p = p.parent;
	}
	return p;
}
////

Both are O(log n).


