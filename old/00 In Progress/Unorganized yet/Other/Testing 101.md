Testing:
========
A) Unit testing:
----------------
Step 1: Create a class for testing.
Step 2: Create a method inside to test a specific function.
Step 3: Write comments to describe expected input and expected output for different cases of the function to be tested.
Step 4: Now write code to test each case.
	- If tests fail, throw exceptions instead of stdout.
	
Step 5: Go to the main (or a function where you expect the tested function to run), make an instance of the class, and run the test.
Step 6: do that for all functions to be tested, encapsulating all tests in a class.
Step 7: make a function or a class that generates random instances of the function input so that you cover as much test cases and corner cases as possible, and use them in testing.
e.g. you can create BST by creating a function that takes integer inputs and creates or adds to an existing BST, then randomize the integers before you input them, creating random BSTs.

B) Testing functions in relation to one another.


