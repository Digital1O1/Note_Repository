# Two categories of C functions that modify head pointer of Linked Lists 

## [Reference link](https://www.geeksforgeeks.org/how-to-write-functions-that-modify-the-head-pointer-of-a-linked-list/)


## 1) Functions that DON'T modify the head pointer
##  Examples :
- Printing linked lists 
- Updating data member of nodes 
  - Example 
    1)  Adding value to all nodes
    2)  Operations that access/update node data

<br>

## Coding example : adds 'x' to data memebers of all nodes
<br>

```cpp

void addXtoList(struct Node *node, int x)
{
    while(node != NULL)
    {
        node->data = node->data + x;
        node = node->next;
    }
}   
```

---

<br>

## 2) Functions that modify the head pointer


## Examples :
- Inserting a node at the beginning 
  - Head pointer ALWAYS modified in this function
- Inserting a node at the end
  - Head pointer modified ONLY when the first node is being inserted
- Deleting a given node
  - Head ponter modified when the deleted node is the first node
---

## Why global variables are bad
<br>

### Reference : http://wiki.c2.com/?GlobalVariablesAreBad

<br>

## Why tho?
1) Non-locality
   - Code easiest to read when the scope of it's elements are limited 
   - Global variables can be read/modified in any part of the program
     - Can make troubleshooting more difficult
2) No access control or constraint checking
   - Global variables can be set/get by any part of the program
   - Any rules regarding it's use can be broken/forgotten
3) Concurrency issues
   - Globals can be accessed by multiple threads of execution and synchronization is needed
   - When dynamically linking modules with globals, the system might NOT be thread-safe
4) Namespace pollution
   - Global namesa re available everywhere
   - You could be using a global whe nyou think you'reu sing a local or vice versa 
5) Memory allocation issues
   - Some environments have memory allocation schemes that make allocation of globals tricky
   - Especially true in languages where constructors have side-effects other than allocation
   - When dynamically linking modules, it's hard to tell which library has their own instances of globals and which ones are shared
  
---

<br>

## Pratical problem to update the head pointer
  - “Given a linked list, write a function deleteFirst() that deletes the first node of a given linked list. For example, if the list is 1->2->3->4, then it should be modified to 2->3->4”
 - Solving this algorithm is a 3 step process
    1) Store the head pointer 
    2) Change the head pointer to point to the next node 
    3) Delete the previous head node. 

<br>

## Here's TWO methods on how to update the head pointer in deleteFirst()
### 1) Returning the head pointer
<br>

```cpp
struct Node *deleteFirst(struct Node *head)
{
    if(head != NULL)
    {
       // store the old value of head pointer
       struct Node *temp = head;
 
       // Change head pointer to point to next node
       head = head->next;
 
       // delete memory allocated for the previous head node
       free(temp);
    }
 
    return head;
}
```
## MAKE SURE YOU ASSIGN THE RETURNED VALUE TO THE HEAD

```cpp
head = deleteFirst(head); // proper use of deleteFirst()
deleteFirst(head); // improper use of deleteFirst(), allowed by compiler
```

## 2) Using a double pointer
 - This method follows a simple C rule
   - If you want to modify a loval variable of one function inside another 
   - Pass a pointer to that variable 
   - So we can pass the pointer to the head pointer 
     - To modify the head pointer in the deletefirst() function

```cpp
void deleteFirst(struct Node **head_ref)
{
    if(*head_ref != NULL)
    {
       // store the old value of pointer to head pointer
       struct Node *temp = *head_ref;
 
       // Change head pointer to point to next node
       *head_ref = (*head_ref)->next;
 
       // delete memory allocated for the previous head node
       free(temp);
    }
}
```
