Linked lists vs. arrays
Arrays have O(N) insertion time, you would have to shift everything over.
Linked lists nodes are not contiguous memory, just need to adjust two next pointers. O(1).
current == nullptr you've reached the end.

```
#include <string>
#include <iostream>

class Node{
  public: 
    std::string val;
    Node* next;

  Node(std::string initialVal){
    val = initialVal;
    next = nullptr;
  }
};

void printList(Node* head){
  Node* current = head;

  while(current != nullptr){
    std::cout << current->val << std::endl; 
    current = current->next;
  }
}

void run() {
  Node a("A");
  Node b("B");
  Node c("C");
  Node d("D");

  a.next = &b;
  b.next = &c;
  c.next = &d;

  printList(&a);
}

void printListRecursive(Node* node){
  if (node == nullptr){ return; }

  std::cout << node->val << std::endl;
  printListRecursive(node->next);
}
```



