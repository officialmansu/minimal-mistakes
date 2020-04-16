---
title: "[Data Structure] Singly Linked List"
toc: true
toc_label: "Table of Contents"
categories:
  - Data Structure
tags:
  - Linked List
---

## 개요

Linked List의 특징에 대해 알아보자. 다음 그림을 보자.

Array의 구조:

![image-20200413004705259](https://officialmansu.github.io/assets/img/array.svg)



Linked List의 구조:![image-20200413004908524](https://officialmansu.github.io/assets/img/linked-list.svg)

Linked List에 대해 알아보기 전, 이를 사용하는 이유에 대해 생각해보자. 그 이유는 기존에 사용하던 방식인 Array와의 차이점을 보면 알 수 있을 것 같다.

## Array와 Linked List 비교하기

Array의 경우, 한번 만들면 크기 변경이 어려우나(정적) Linked List의 경우 값을 추가하거나 삭제하면 크기가 변할 수 있다.(동적)

Array의 경우, 값은 메모리에 순서대로 저장되나, Linked List는 값을 메모리에 무작위적으로 저장한다. 이러한 이유로 Array는 index로 값을 바로 접근할 수 있지만 Linked List는 처음 Node부터 index까지 거쳐서 접근해야 한다.

또, Array는 데이터 타입의 크기의 개수만큼 메모리에 할당되지만, Linked List의 경우, 한 데이터(Node)가 추가될 때마다 Node의 크기만큼 메모리에 추가 할당된다.

또한, Array의 경우 값을 삭제하거나 특정 index에 삽입하는 동작은 작업을 수행하려는 위치의 오른쪽에 있는 원소의 개수만큼 시간이 소요된다. 하지만 Linked List의 경우 Node간 전후 연결을 끊고 새로운 Node에 전후 Node의 메모리 주소를 저장하면 되므로 항상 일정한 시간이 소요된다.

## Singly Linked List 만들어 보기

Linked List는 Node로 구성된다. Node는 data와 다음 Node의 메모리 주소(또는 참조)를 저장한다. C언어로 구현해보자.

```c
struct Node {
  int data;
  struct Node* next;
};
```

Node를 만들었다. 다음은 간단한 Linked List를 만들어보자.

![linked-list-svg](https://officialmansu.github.io/assets/img/linked-list-svg.svg)

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};

int main(void) {
    struct Node* head = (struct Node*)malloc(sizeof(struct Node));
    struct Node* second = (struct Node*)malloc(sizeof(struct Node));
    struct Node* third = (struct Node*)malloc(sizeof(struct Node));
    
    // link head to second
    head -> data = 1;
    head -> next = second;
    
    // link second to third
    second -> data = 2;
    second -> next = third;
    
    // link third to NULL pointer
    third -> data = 3;
    third -> next = NULL;
    
    return 0;
}
```

다음 글에서는 Linked List를 가지고 여러가지 실험을 해 볼 것이다.