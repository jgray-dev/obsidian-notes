#### Data structure
A concrete method for storing a collection of data.


##### Dictionaries/Arrays
Most languages these are both a data type, and a data structure
**Most interview questions will involve iteration through these types**


##### Stacks and queues (arrays)

Stack - Last in, first out
- Like a undo/redo feature. When we make a change, the first thing we "undo" in the most recent change. The more we "undo", the further back in time we go (deeper into the stack)
Queue - First in, first out
- Queues exactly like you expect, whoever got there first, goes first


##### Linked lists

Using multiple arrays to make processes like insertion easier.


Example of a **singly** linked list:

| Memory address | value | pointer   |
| -------------- | ----- | --------- |
| 0x00           | 1     | 0x01      |
| 0x01           | 3     | 0x02      |
| 0x02           | 4     | 0x03      |
| 0x03           | 5     | 0x04      |
| 0x04           | 6     | . . . . . |


**Given a singly linked list and an integer K, remove the K-th last element from the list. K is guaranteed to be smaller than the length of the list.**

*Original thought: (not entirely correct)
if k = 2, keep track of whatever element we passed 2 elements ago
old = array of length 2
old[0] = 2 elements ago
old[1] = 1 element ago
old[0] = current element

*if current element is the end of the linked list, remove the element stored at memory location old[0]
if current element is NOT end of linked list, old[0] = old[1], old[1] = old[2], old[2] = current element

Final thought: (**solution**)
if k = 3, keep track of whatever element we passed 3 elements ago
save listK as the element of the linked list that we just passed, if the one we're looping over is the end, return listK, if its not, go through listK and set it equal to whatever pointer is listK.next
```python
head = linkedlist.head # (first head)
headK = linkedlist.head # (second head [Kth last element])
count = 0

loop linked list here:
	if count >= k:
		if head.next == undefined:
			return headK # (REMOVE headK element HERE)
		else:
			head = head.next
			headK = headK.next
	else:
		count+=1
		head = head.next
```





To insert in a traditional array, we must "shift", the numbers that come after
to insert 2 between 1 and 3 - 3, 4, 5, 6 must all me moved

In a linked list, we can change the pointer of value 1, to our new value's memory location

| Memory address | value | pointer   |
| -------------- | ----- | --------- |
| 0x00           | 1     | *0x05*    |
| 0x01           | 3     | 0x02      |
| 0x02           | 4     | 0x03      |
| 0x03           | 5     | 0x04      |
| 0x04           | 6     | . . . . . |
| 0x05           | 2     | *0x01*    |
Modify the value 1 pointer to show our address for value 2, and change our value 2 pointer to whatever the original pointer was for value 1


We also have **doubly** linked lists, where instead of only storing the *next* memory address, we set a pointer to both the next and previous memory address


| Memory address | value | pointer_next | pointer_previous |
| -------------- | ----- | ------------ | ---------------- |
| 0x00           | 1     | 0x01         | undefined (head) |
| 0x01           | 2     | 0x02         | 0x00             |
| 0x02           | 3     | 0x03         | 0x01             |
| 0x03           | 4     | 0x04         | 0x02             |
| 0x04           | 5     | .0x05        | 0x03             |
| 0x05           | 6     | . . . . .    | 0x04             |
so now we're able to "travel" backwards in a doubly linked list, where in a singly linked list we would be unable to because we only have pointers forward



#### Trees
Typically used for efficient searches

Binary tree - Each node will *only* ever have 2 children

Balanced tree - 
All the values to the left of a selected node will be less than the selected node. 
All the values to the right of a selected node will be greater than the selected node.
![[balanced.png]]



Unbalanced tree - values are basically random 



![[NOT BALANCED.webp]]


****
Normal jobs should stop studying here, but if you're going for Google or Amazon, continue here:



### Graphs

A loose collection of nodes and the paths between them
![[graphs.png]]

A weighted + dense model is a very very simple visualization of a large language model. You give it a certain input, it uses its weights to determine a specific path to take, and eventually get an output.

Another use case would be pathfinding. You have a bunch of nodes "locations" and a bunch of weights (can correspond to length of a road, travel time, etc). Even though one path may be only 2 nodes away, if the weight required for those nodes is too low (high weight = preferred road), a route with 4 nodes that have better weights may be more efficient.
![[weighted graph.png]]
To get from point `F` to point `I`, logic may tell you to go `F=>E=>H=>I`, but in reality, taking the path `F=>G=>N=>J=>I` may be a shorter route, despite having more nodes, the weights associated with these nodes are preferred





