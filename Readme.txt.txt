This project took the base code built throughout the Codecademy lesson "Binary Search Trees: Python" (available at https://www.codecademy.com/courses/learn-data-structures-and-algorithms-with-python/lessons/binary-search-tree-python/exercises/review). A Binary Search Tree is a tree-shaped data structure where any node can have a left and a right child and in which the nodes are placed according to their values, on the left of the higher-level nodes with a higher value than them, on the right of those with a lower or equal value. To note that with such structure desequilibrium of the tree can be a challenge, i.e. a tree that maximizes its depth but not its breadth due to improper sequencing of the nodes added to it (too much linearity down or up leading to add one level more at each addition).


What I did is strive to add to the initial code, which can be consulted in the "start_code" branch, the improvements outlined at the end of the course (add a delete method+add a function to balance a desequilibrated tree), as well as additional improvement features. 

Should be stressed the interconnection between some of the added features, the features holding relationships with each other will be treated and explained together in this readme.

Added features:

- .__repr__() function, which provides a vertical representation of the tree's nodes with their values and indication of their level given by the number of ">" characters, example:

> 28
> > 25
> > > 12
> > > > 1
> > > > 17
> > > > > 22
> > 35
> > > 29
> > > > 32
> > > 46
> > > > 55


- .remove_node_plus_deps() function, the simplest modality of deleting from a tree as it is just about removing a given node based on its value plus all the potential dependencies and subdependencies of this node: without therefore bothering to rebuild into the tree the all structure that was under the node flagged for deletion. This function uses a recursive approach, calling itself on the current node's left node or right node depending on how current node's value relates to the target value. It prints a message when the target node is found to confirm the deletion and mention the level at which it happened. An example of such operation below.

BEFORE

> 55
> > 54
> > > 51
> > > > 50
> > > > 52
> > > > > 53
> > 58
> > > 56
> > > > 57
> > > 59
> > > > 60

AFTER (removing node with value 54 plus dependencies)

> 55
> > 58
> > > 56
> > > > 57
> > > 59
> > > > 60


- .remove_single() function, thanks to which this time we only remove the node that has the targeted value but all the nodes that are under it (this node's subtree, in a way), get re-added into the tree. This is done by articulating three functions: 

		1) the .dependencies() function, an other addition from ours that itself looks for the target node in the first place and then using the return of another added 		                	           function .tree_values_inorder() returns all the values under the target node in the form of a list sorted in ascending order
                2) the .remove_node_plus_deps() function that we just described, that removes from the tree the target node plus its dependencies
		3) the .insert_list() or better the .insert_list_pointers() functions which add the values retrieved in step 1) back into the tree. These functions work by taking a sorted list as 		   an input, singling out the median value of the list as the current node to add, and then recursing on the sublists that are on the left and the right of this median value. This 		   approach allows to operate in the right sequence median-left-right values and therefore to make the best of each level of the tree. The .insert_list_pointers() is an optimized 		   solution in that it uses pointers to operate on the same list all the time, instead of creating at each step copies of subsets of the base list, as .insert_list() does.

An example:

BEFORE

> 55
> > 54
> > > 51
> > > > 50
> > > > 52
> > > > > 53
> > 58
> > > 56
> > > > 57
> > > 59
> > > > 60

AFTER (removing single node 54)

> 55
> > 51
> > > 50
> > > 52
> > > > 53
> > 58
> > > 56
> > > > 57
> > > 59
> > > > 60

- .rebalance_tree() function, which is working quite similarly as .remove_single(), except that it takes all the existing values in the tree in a sorted fashion, makes the median value the new root node and performs the insert_list()/insert_list_pointers() process on the rest of the nodes. Concretely, this function can accomplish the rebalance shown below, reducing the depth of a tree from 11 to 5 levels.

BEFORE

> 50                                                          
> > 51
> > > 52
> > > > 53
> > > > > 54
> > > > > > 55
> > > > > > > 56
> > > > > > > > 57
> > > > > > > > > 58
> > > > > > > > > > 59
> > > > > > > > > > > 60         

AFTER

> 55
> > 54
> > > 51
> > > > 50
> > > > 52
> > > > > 53
> > 58
> > > 56
> > > > 57
> > > 59
> > > > 60      


- .depth_first_traversal_inorder(), a function that is going through a tree's nodes in increasing order and prints a message with each node's depth and value, output of this function for the tree shown just above:

Depth=4, Value=50
Depth=3, Value=51
Depth=4, Value=52
Depth=5, Value=53
Depth=2, Value=54
Depth=1, Value=55
Depth=3, Value=56
Depth=4, Value=57
Depth=2, Value=58
Depth=3, Value=59
Depth=4, Value=60

- .depth_first_traversal_preorder(), a function that delivers the same type of output but proceeds recursively according to sequence middle-left-right. So it prints info on the current node, goes as far left as it can find left nodes, when it no longer can starts to deliver info on the right nodes. Below the output of this function for the above tree, with some comments:

Depth=1, Value=55
Depth=2, Value=54
Depth=3, Value=51
Depth=4, Value=50 ---> not possible to go more on the left as this node has no left node
Depth=4, Value=52 ---> first node on the right of something to ever be printed
Depth=5, Value=53
Depth=2, Value=58 ---> left part of the tree fully traversed so starting to deliver info about the right part
Depth=3, Value=56
Depth=4, Value=57
Depth=3, Value=59
Depth=4, Value=60

- .depth_first_traversal_postorder(), proceeds recursively according to sequence left-right-middle, output for our example tree with comments:

Depth=4, Value=50 ---> leftmost node, no longer possible to continue this way
Depth=5, Value=53 ---> therefore continuing by looking on the right of node 51 (valid for this line and the below too)
Depth=4, Value=52
Depth=3, Value=51 
Depth=2, Value=54
Depth=4, Value=57 ---> the traversal of the tree's left part is finished, starting to traverse its right part, prioritizing the left direction again
Depth=3, Value=56
Depth=4, Value=60
Depth=3, Value=59
Depth=2, Value=58
Depth=1, Value=55 ---> root node printed at last


              