---
title: "[Data Structure] 실험 2_Singly Linked List의 값 삭제하기"
toc: true
toc_label: "Table of Contents"
categories:
  - Data Structure
tags:
  - Linked List
---

이번에는 Singly Linked List에 있는 값을 삭제하는 실험을 해보자.

## 실험 2_값 삭제하기

먼저 값을 삭제하는 방법은 크게 2가지로 구분된다.

1. 값으로 검색해 처음 나오는  Node삭제
2. 위치(`index`)로 주어진 Node삭제

### 실험 2의 1_검색 삭제

값으로 검색해 삭제하는 방법은 2가지로 구분된다.

1. 중간 Node또는 마지막 Node삭제하기
2. 맨 앞 Node삭제하기

이렇게 2가지로 나누게 된 이유는 만들면서 알게 될 것이며, 이 방법을 서로 다른 함수로 만들지 않고 하나의 함수로 구성할 것이다. 이는 사고 과정일 뿐이다.

#### 중간 Node또는 마지막 Node삭제하기

Node를 삭제하려면 먼저 삭제하려는 Node의 전 Node와 검색할 값이 필요하다. 그 이유는 검색할 값이 저장된 Node를 연결고리에서 끊어내고 그 전 Node의 다음 Node를 끊어낸 Node의 다음 Node로 하면 되기 때문이다.

![singly-linked-list-delete-at-middle](https://officialmansu.github.io/assets/img/singly-linked-list-delete-at-middle.svg)

이를 위해서는 `head` 포인터와 검색할 값이 필요하다.

```c
void removeNode(struct Node* head, const int data) {
    struct Node* prev_node = NULL; // 삭제하려는 Node의 전 Node
    // head != NULL: List가 텅 비어있거나 검색할 값이 없는 경우
    while(head != NULL && head -> data != data) {
        prev_node = head;
        head = head -> next;
    }
    // List가 비었거나 검색할 값이 없는 경우 함수 종료
    if(head == NULL) return;
    prev_node -> next = head -> next;
    free(head);
}
```

#### 맨 앞 Node삭제하기

위 방법으로 맨 앞 Node를 삭제하려고 하면 세그멘테이션 오류가 발생한다. 허용되지 않은 메모리 영역에 접근했거나 허용되지 않은 접근 방법을 사용했다는 것이다. (8줄의 `prev_node` 접근 불가)따라서 새로운 방법이 필요하다. 다음 그림을 참고하자.

![singly-linked-list-delete-at-front](https://officialmansu.github.io/assets/img/singly-linked-list-delete-at-front.svg)

먼저 [맨 앞에 값 삽입하기](https://officialmansu.github.io/data%20structure/singly-linked-list-insert/#%EC%8B%A4%ED%97%98-1%EC%9D%98-1_list%EC%9D%98-%EB%A7%A8-%EC%95%9E%EC%97%90-%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0)에서 사용된 2중 포인터의 개념을 도입해야 한다. 그리고 `head` 포인터를 임시로 저장할 임시 포인터 변수가 필요하다. 그 이유는 `head` 포인터가 가리키는 Node를 바로 다음 Node로 바꾸어야 하며, 삭제할 Node를 검색해야 하기 때문이다.

```c
void removeNode(struct Node** head, const int data) {
    struct Node* prev_node = NULL;
    struct Node* target = *head; // 검색용, 맨 앞의 Node를 삭제(free)하기 위함
    // head_tmp == NULL: List가 비었거나 검색할 값이 없는 경우
    while(target != NULL && target -> data != data) {
        prev_node = target;
        target = target -> next;
    }
    // List가 비었거나 검색할 값이 없는 경우 함수 종료
    if(target == NULL) return;
    // 맨 앞 Node삭제 후 함수 종료
    if(prev_node == NULL) {
        *head = (*head) -> next;
        free(target);
        return;
    }
    // 중간 Node또는 마지막 Node삭제하기
    prev_node -> next = target -> next;
    free(target);
}
```

### 실험 2의 2_위치 삭제

위치 삭제는 삭제할 Node가 배열의 `index`처럼 주어지고 이를 삭제하는 것을 말한다. Node를 삭제하는 방법은 실험 2의 1과 같다. 다만 `index`만큼 반복해 값을 찾고 이를 삭제하면 된다.

```c
void removeNode(struct Node** head, const int index) {
    int i;
    struct Node* prev_node = NULL;
    struct Node* target = *head;
    for(i = 0; target != NULL && i < index; ++i) {
        prev_node = target;
        target = target -> next;
    }
    if(target == NULL) return;
    if(prev_node == NULL) {
        *head = (*head) -> next;
        free(target);
        return;
    }
    prev_node -> next = target -> next;
    free(target);
}
```

## 실험 3_List 삭제하기

List를 삭제하기 위해서는 `head` 포인터의 포인터가 필요하다. 이중 포인터를 사용하는 이유는 다음 그림을 보면 이해할 수 있다.

![delete-linked-list](https://officialmansu.github.io/assets/img/delete-linked-list-1587989183510.svg)

이와 같이 `free(current_node)`를 한 뒤, 그 다음 Node를 지우려면 `current_node`를 지우기 전, `next_node`라는 이름으로 다음 Node를 미리 저장해 두어야 한다.

```c
void deleteList(struct Node** head) {
  struct Node* current_node = *head;
  struct Node* next_node;
  while(current_node != NULL) {
    // 다음 Node 미리 저장
    next_node = current_node -> next;
    // 현재 Node 해제
    free(current_node);
    // 
    current_node = next_node;
  }
  *head = NULL;
}
```

__이 글에 잘못된 내용이 있을 경우 알려주시기 바랍니다.__