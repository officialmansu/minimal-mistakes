---
title: "[Data Structure] 실험 1_Singly Linked List에 값 추가하기"
toc: true
toc_label: "Table of Contents"
categories:
  - Data Structure
tags:
  - Linked List
---

저번 글에서는 Singly Linked List가 무엇인지, Array와 어떻게 다른지를 알아보고 만들어 보았다. 이제는 그 Singly Linked List를 가지고 여러 실험을 해 볼 것이다.

## 실험 1_값 추가하기

값을 추가하는 방법은 크게 3가지가 있다.

1. List의 맨 앞에 추가하기
2. 특정 Node의 앞에 추가하기
3. List의 맨 끝에 추가하기

### 실험 1의 1_List의 맨 앞에 추가하기

해당 함수의 형태를 생각해보자. Node 포인터와 추가할 값을 매개변수로 해보자. 반환 값은 `void` 이다.

```c
void insertFront(struct Node* node, const int data) {
	// insert code here.
}
```

먼저 새로운 Node를 맨 앞에 추가해야 하는데, 그러기 위해서는 head를 임시 Node에 복사하고 head에는 매개변수로 들어온 `data`를 저장한다.

![singly-linked-list-insert-front](https://officialmansu.github.io/assets/img/singly-linked-list-insert-front.svg)

```c
void insertFront(struct Node* head, const int data) { // 검증
	struct Node* head_tmp = (struct Node*)malloc(sizeof(struct Node));

	// head를 head_tmp에 복사
	head_tmp -> data = head -> data;
	head_tmp -> next = head -> next;

	// 새 data를 head에 저장
	head -> data = data;
	// head_tmp를 head의 다음 Node로 지정
	head -> next = head_tmp;
}
```

굳이 `head`를 임시 Node에 복사해, `head`의 메모리 주소를 변하지 않게 하는 이유는 해당 `insertFront`함수를 다시 호출할 수도 있기 때문이다.

### 실험 1의 2_특정 Node의 앞에 추가하기

해당 함수의 형태를 생각해보자. 먼저 매개변수로는 우리가 추가할 위치의 바로 전 Node의 포인터와 새로 추가할 값으로 생각할 수 있다. 반환 값은 `void` 이다.

```c
void insert(struct Node* prev_node, const int data) {
	// insert code here.
}
```

먼저 새로운 `data`를 새 Node에 저장한다. 그리고 새 Node의 다음 Node를 `prev_node` 의 다음 Node로 한다. 또 `prev_node`의 다음 Node는 새 Node로 한다.

![singly-linked-list-insert](https://officialmansu.github.io/assets/img/singly-linked-list-insert.svg)

```c
void insert(struct Node* prev_node, const int data) {
	struct Node* node = (sizeof(struct Node))malloc(sizeof(struct Node));
  
	node -> data = data;
	node -> next = prev_node -> next;
  
	prev_node -> next = node;
}
```

### 실험 1의 3_List의 맨 끝에 추가하기

해당 함수의 형태를 생각해보자. 매개변수로는 한 Node의 포인터, 새로 추가될 `data`라고 할 수 있다. 반환 값은 `void`이다.

```c
void append(struct Node* node, const int data) {
	// insert code here.
}
```

매개변수로 어떤 Node가 들어오든 그 Node가 포함된 List의 마지막 Node를 찾고 앞서 실험 1의 2에서 만들었던 함수를 호출하면 된다.

```c
void append(struct Node* node, const int data) {
	while(node -> next != NULL) {
		node = node -> next;
	}
	insert(node, data);
}
```

지금까지 Singly Linked List에 값을 추가하는 여러 방법에 대해 실험해 보았다. 다음에는 값을 삭제하는 방법에 대해 실험해보자.