# iterative preorder tree traversal

1) create an empty stack and push root note to stack
2) while stack isn't empty
....a) pop an item from stack and process it
....b) push the right child of this item into stack
....c) push the left child of this item into stack

# iterative inorder tree 

1) create an empty stack s  
2) initialize current node as root  
3) push the current node to s and set current = current->left until current is NULL
4) if current is NULL and stack not empty then  
....a) pop the top item from stack  
....b) print the popped item, set current = popped_itme->right  
....c) go to 3)  
5) if current is NULL, and stack is empty then we are done  

# iterative postorder tree traversal

## method 1: with two stack

1) push root into first stack  
2) loop while first stack is not empty  
....a) pop a node from first stack and push it into second stack  
....b) push left and right children of the popped node to first stack  
3) print contents of second stack  

pushing and popping into first stack is just like the stack in preorder traversal except the order is reversed.  
second stack is just to reverse its order

## method 2: with one stack

1) create an empty stack  
2) do followings while root is not NULL  
....a) push root's right child and then root to stack  
....b) set root as root's left child  
3) pop an item from stack and set it as root  
....a) if popped item has a right child and the right child  
....   is at top of stack, then remove the right child from stack,  
....   push the root back and set root as root's right child  
....b) else print root's data and set root as NULL  
4) repeat 2) & 3) until stack is empty  

## method 3: reverse preorder traversal
reverse the direction of preorder tree traversal  
save item into a temporary space, and push left child then right child into stack  
before finish reverse the whole temporary space

# Reverence
https://www.geeksforgeeks.org/iterative-preorder-traversal/  
https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/  
https://www.geeksforgeeks.org/iterative-postorder-traversal/  
https://www.geeksforgeeks.org/iterative-postorder-traversal-using-stack/  
