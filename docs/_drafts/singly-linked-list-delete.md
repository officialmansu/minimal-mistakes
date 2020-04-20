이번에는 Singly Linked List에 있는 값을 삭제하는 실험을 해보자.

/*temp code*/

```c
#include <stdio.h>

#include <stdlib.h>



struct Node {

  int data;

  struct Node* next;

};



void printList(struct Node* head) {

  while(head != NULL) {

    printf("%d ", head -> data);

    head = head -> next;

  }

  printf("\n");

}



void insertFront(struct Node** head, const int data) {

  struct Node* node = (struct Node*)malloc(sizeof(struct Node));

  node -> data = data;

  node -> next = *head;



  *head = node;

}



void insert(struct Node* prev_node, const int data) {

  struct Node* node = (struct Node*)malloc(sizeof(struct Node));

  node -> data = data;

  node -> next = prev_node -> next;



  prev_node -> next = node;

}



void append(struct Node** node, const int data) {

  if(*node == NULL) {

    insertFront(node, data);

    return;

  }

  struct Node* last = *node;

  while(last -> next != NULL) {

    last = last -> next;

  }

  insert(last, data);

}



void removeNode(struct Node** head, const int data) {
  struct Node* prev_node = NULL;
  struct Node* head_tmp = *head; // 검색용, 맨 앞의 Node를 삭제(free)하기 위함
  while(head_tmp != NULL && head_tmp -> data != data) {
    prev_node = head_tmp;
    head_tmp = head_tmp -> next;
  }
  if(head_tmp == NULL) return; // if list has no value or key is not in list:
  if(prev_node == NULL) { // delete node at front
    *head = (*head) -> next;
    free(head_tmp);
    return;
  }
  prev_node -> next = head_tmp -> next;
  free(head_tmp);
}



int main(void) {

  struct Node* head = NULL;

  int i;

  for(i = 0; i < 10; i++) {

    append(&head, i + 1);

  }



  printList(head);



  return 0;

}
```