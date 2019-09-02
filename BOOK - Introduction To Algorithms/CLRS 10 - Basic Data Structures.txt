Elementary Data Structures:
---------------------------
Data structures usually use pointers, that's the key to their creation.
------------------------------------------------------------------------------------------------------------------------------------------------
A) Stacks & Queues:
-------------------
Stack is a dynamic set where, when we remove an element from that set, we should remove the element that was last inserted into the set i.e. LIFO.
Queue is a dynamic set where, when we remove an element from that set, we should remove the element that was first inserted into the set i.e. FIFO.

There are a ton of efficient implementations for both.

Stack:
------
It can be implemented using an array and an attribute called top where it's an index of the last inserted element.
////
class stack 
{
public:
	stack(int size)
	{
		stackSize = size;
		s = new int[stackSize];
	}
	
	~stack()
	{
		delete[] s;
	}
	
	bool isEmpty()
	{
		return top == 0;
	}
	
	void push(int i)
	{
		if(top == stackSize)
			throw exception overflow;
		top++;
		s[top] = i;
	}
	
	int pop()
	{
		if(top == 0)
			throw exception underflow;
		top--;
		return s[top + 1];
	}
	
private:
	int top = 0;
	int* s;
	int stackSize;
}
////

Basic operations:
	Push(element e): inserts element.
	Pop(): removes the element that was last inserted, regardless of what it is.
	isEmpty(): true if stack has no elements.
	
Stack underflow: trying to pop from an empty stack.
Stack overflow: trying to push onto a full stack.

Queue:
------

////
class queue 
{
public:
	queue(int size)
	{
		queueSize = size;
		q = new int[queueSize];
	}
	
	~queue()
	{
		deete[] q;
	}
	
	bool isEmpty()
	{
		return head == tail;
	}
	
	void enqueue(int i)
	{
		if(head = tail + 1)
			throw exception queue overflow;
		tail = (tail + 1) % queueSize;
		q[tail] = i;
	}
	
	int dequeue()
	{
		if(isEmpty)
			throw exception underflow;
		int temp = head;
		head--;
		if(head == 0)
			head = queueSize - 1;
		return q[temp];
	}
	
private:
	int head = 0;
	int tail = 0;
	int* q;
	int queueSize;
}
////
------------------------------------------------------------------------------------------------------------------------------------------------
B) Linked lists:
----------------
Linked lists are good as dynamic sets, not the best thing in efficiency.

In a doubly linked list, each element consists of three things: value, next*, prev*.
If prev = NULL, that element is the head.
If next = NULL, that element is the tail.
If head = NULL, the list is empty.

Singly linked list: we have the next* pointer only;
Sorted linked list: list is sorted by the data inside nodes;
Circular linked list: tail's next* points to head and head's prev points to tail.

////
class node
{
public:
	node();
	
	node* next()
	{
		return next;
	}
	
	node* prev()
	{
		return prev;
	}

private:
	int data;
	node* next;
	node* prev;
}
////

////
class list
{
public:
	//stuff
	
private:
	node* head;
	node* tail;
}
////

Basic operations:
	Searching a list: since we can't random access the nodes, we have no option other than linear search O(n). (Can be O(n/2) if we use both pointers).
	Inserting at the front: O(1).
	Inserting at the back: O(1) for doubly, O(n) for singly.
	Inserting at any place: O(n/2) for doubly, O(n) for singly.
	Deleting from any place: O(n/2) for doubly, O(n) for singly. (It uses linear search to get a pointer to the element, then performs the delete operation)
	
////
	node* search(int data) //O(n)
	{
		node* x = head;
		for(; x->next != NULL; x = x->next())
		{
			if(x->data = data)
				return x;
		}
		return NULL;
	}
	
	node* searchFaster(int data) //O(n/2)
	{
		node* x = head;
		node* y = tail;
		while(x != y)
		{
			if(x->data = data)
				return x;
			if(y->data = data)
				return y;
			head = head->next();
			tail = tail->prev();
		}
		return NULL;
	}
	
	void delete(node* x)
	{
		if(x->prev() != NULL)
			x->prev()->next() = x->next();
		else 
			head = x->next();
		if(x->next()->prev() != NULL)
			x->next()->prev() = x->prev(); 
	}
////

Now, imagine we omit the boundary conditions of delete():
////
void delete(node* x)
{
	x->prev()->next() = x->next();
	x->next()->prev() = x->prev();
}
////

That is possible if we use a Sentinel.

Sentinels: dummy objects used to simplify boundary conditions.

If we make an extra node called NIL and put it after the tail and before the head (Making the list circular), whenever we have a reference to NULL we replace it with the node NIL.

The list is physically circular, but we treat it normally since we know that the node NIL refers to the end/beginning of the list, it's as if we made all dangling references point to the same NULL, then put that NULL in an object so as to cycle through the list.

Usually sentinels are used only when they REALLY simplify the code, as you can see it's a real object that takes memory.

------------------------------------------------------------------------------------------------------------------------------------------------------------
C) Implementing pointers and objects:
-------------------------------------
How do we implement pointers and objects in languages that don't have them?

1- Multiple arrays for objects:
-------------------------------
Idea: represent objects that have the same attributes by using an array per attribute, for example we can use three arrays to represent linked lists:
////
int[] next = {/, 0, 1}
int[] data = {3, 4, 5}
int[] prev = {1, 2, /}
////
Where this NULL<=>[5]<=>[4]<=>[3]<=>NULL 

For an index i, we can get the key, next, and prev, from the three arrays.
The pointers next, prev, etc. are just common indices like the 1 since that's the node in the middle.

You can also use ONE array, to store many of the same object, in this case we can represent the node of a linked list as three consecutive places in an array.
////
int[] LL = {0, 0, 0, 4, 7 ,13, 1, /, 4, 0, 0, 0, 16, 4, 19, ...}
////
Where a node is [16, 4, 19], 16 is key, 4 is next, 19 is prev.

2- Allocating and freeing objects:
----------------------------------
It's useful to keep track of objects not currently used so that we can allocate them if we need to.

Garbage collection is responsible for that.

------------------------------------------------------------------------------------------------------------------------------------------------------------
D) Representing rooted and binary trees:
----------------------------------------
How can we represent trees using linked data structures?

