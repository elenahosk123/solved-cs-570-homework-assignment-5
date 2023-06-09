Download Link: https://assignmentchef.com/product/solved-cs-570-homework-assignment-5
<br>
2 Assignment

A treap is a binary search tree (BST) which additionally maintains heap priorities. An example is given in Figure 1. A node consists of

• A key k (given by the letter in the example),

• A random heap priority p (given by the number in the example). The heap priority p is assigned at random upon insertion of a node. It should be unique in the treap.

• A pointer to the left child and to the right child node.

Note that if you insert just the keys into an empty BST following the priority (from max to min) you get back a BST of the exact same form as the treap. For example, if you insert the keys h, j, c, a, e into an empty BST, then you get back a tree just like that in Figure 1. Since the priorities are chosen at random, the resulting tree is one particular permutation (in this case [h,j,c,a,e]) chosen at random. Thus treaps may be understood as random BSTs.Regarding the performance of its operations, on average (the run time depends on the randomly chosen heap priorities), add, delete, and find operations take O(log(n)) time where n is the number of nodes in the treap.

Figure 1: Example of a Treap

In this homework, you will implement a treap. In the following, we discuss the components of the data structure and its operations in detail.

2.1 The Node Class

Create a private static inner class Node&lt;E&gt; of the Treap class (described in the next subsection) with the following attributes and constructors:

• Data fields:

p u b l i c E data // key for the search p u b l i c int priority // random heap priority p u b l i c Node &lt;E &gt; left p u b l i c Node &lt;E &gt; right

24• Constructors:p u b l i c Node ( E data , int priority )

Creates a new node with the given data and priority. The pointers to child nodes are null. Throw exceptions if data is null.

• Methods:

Node &lt;E &gt; r o t a t e R i g h t () Node &lt;E &gt; r o t a t e L e f t ()1rotateRight() performs a right rotation according to Figure 2, returning a reference to the root of the result. The root node in the figure corresponds to this node. Update the data and priority attributes as well as the left and right pointers of the involved nodes accordingly.rotateLeft() performs a left rotation according to Figure 2 returning a reference to the root of the result. The root node in the figure corresponds to this node. Update the attributes of the nodes accordingly.Why rotation? Rotations preserve the ordering of the BST, but allows one to restore the heap invariant. Indeed, after adding a node to a treap or removing a node from a treap, the node may violate the heap property considering the priorities. In this case it is necessary to perform one or more of these rotations to restore the heap property. Further details shall be supplied below.

(a) (b)

Figure 2: Right rotation (a→b) and left rotation (b→a)

2.2 The Treap Class

• Data fields:

p r i v a t e Random p r i o r i t y G e n e r a t o r ; p r i v a t e Node &lt;E &gt; root ;2E must be Comparable.

• Constructors:

p u b l i c Treap ()p u b l i c Treap ( long seed )2Treap() creates an empty treap. Initialize priorityGenerator using new Random(). See http://docs.oracle.com/javase/8/docs/api/java/util/Random.html for more in-formationregarding Java’s pseudo-random number generator.Treap(long seed) creates an empty treap and initializes priorityGenerator using new Random(seed).

• Methods:

b o o l e a n add ( E key )b o o l e a n add ( E key , int priority ) b o o l e a n delete ( E key )p r i v a t e b o o l e a n find ( Node &lt;E &gt; root , E key ) p u b l i c b o o l e a n find ( E key )p u b l i c String toString ()2

4

6

We next describe each of these methods.

2.2.1 Add operation

To insert the given element into the tree, create a new node containing key as its data and a random priority generated by priorityGenerator. The method returns true, if a node with the key was successfully added to the treap. If there is already a node containing the given key, the method returns false and does not modify the treap.

• Insert the new node as a leaf of the tree at the appropriate position according to the ordering on E, just like in any BST.• If the priority of the parent node is less than the priority of the new node, bubble up the new node in the tree towards the root such that the treap is a heap according to the priorities of each node (the heap is a max-heap, i.e., the root contains the highest priority). To bubble up the node, you must implement the rotation operations mentioned above.

Here is an example in which the node (i,93) is added to the treap.

Hint: For the add method you should proceed as in the addition for BSTs. Except that I would suggest

• adapting it to an iterative version (rather than using recursion).

• storing each node in the path from the root until the spot where the new node will be inserted, in a stack

• after performing the insertion, use a helper function reheap (with appropriate parameters that should include the stack) to restore the heap invariant. Note that if our nodes had pointer to their parents, then we would not need a stack.

• have add(E key) call the add(E key, int priority) method once it has generated the ran-dom priority. Thus all the “work” is performed by the latter method.2.2.2 Delete operationboolean delete(E key) deletes the node with the given key from the treap and returns true. If the key was not found, the method does not modify the treap and returns false. In order to remove a node trickle it down using rotation until it becomes a leaf and then remove it. When trickling down sometimes you will have to rotate left and sometimes right. That will depend on whether there is no left subtree of the node to delete, or there is no right subtree of the node to erase; if the node to erase has both then you have to look at the priorities of the children and consider the highest one to determine whether you have to rotate to the left or the right.

Here is an example of deletion of the node (z,47):

2.2.3 Find operation

private boolean find(Node&lt;E&gt; root, E key): Finds a node with the given key in the treap rooted atroot and returns true if it finds it and false otherwise. boolean find(E key): Finds a node with the given key in the treap and returns true if it finds itand false otherwise.

2.2.4 toString operationpublic String toString(): Carries out a preorder traversal of the tree and returns a represen-tation of the nodes as a string. Each node with key k and priority p, left child l, and right child r is represented as the string [k, p] (l) (r). If the left child does not exist, the string representation is [k, p] null (r). Analogously, if there is no right child, the string representa-tion of the tree is [k, p] (l) null. Variables l, k, and p must be replaced by its corresponding string representation, as defined by the toString() method of the corresponding object.Hint: You can reuse the exact same method in the binary tree class we saw in class. You will have to add a toString method to the Node class so that it prints a pair consisting of the key and its priority.

3 An Example Test

For testing purposes you might consider creating a Treap by inserting this list of pairs(key,priority) using the method boolean add(E key, int priority):

(4,19);(2,31);(6,70);(1,84);(3,12);(5,83);(7,26)

The code for building this Treap is:

testTree = new Treap &lt; Integer &gt;(); testTree . add (4 ,19); testTree . add (2 ,31);testTree . add (6 ,70); testTree . add (1 ,84);testTree . add (3 ,12); testTree . add (5 ,83);testTree . add (7 ,26);

246

8The resulting Treap should look like this:

The output using toString() of the above would be

( key =1 , priority =84) null( key =5 , priority =83)( key =2 , priority =31)

2

4

null( key =4 , priority =19)( key =3 , priority =12)null nullnull( key =6 , priority =70)null( key =7 , priority =26)null null6

8

10

12

14