---
title: "[Data Structure] Singly Linked List의 길이 구하기"
toc: true
toc_label: "Table of Contents"
categories:
  - Data Structure
tags:
  - Linked List
---

이번에는 Singly Linked List의 Node의 개수를 세는 함수(`length`)를 만들어 보자

## 방법 1_반복문 사용하기

`length`함수의 형태는 `head`포인터를 인자로 하고 `int`를 반환하는 형태로 한다. 그리고 Node들을 순회하면서 `count`를 1씩 증가하면 된다.

```c
int length(struct Node* head) {
    int count = 0;
    while(head != NULL) {
        head = head -> next;
    }
    return count;
}
```

## 방법 2_재귀적으로 표현하기

`length`함수의 형태는 방법 1과 같지만, 반복문이 아닌 재귀적으로 표현한다는 점에서 다르다. 먼저 반환값의 형태는 두 가지로 나누어진다.

- `head`포인터가 `NULL`인 경우: 0
- 그렇지 않은 경우: `1 + length(struct Node* head)`

이렇게 구성할 경우, `head`포인터가 NULL에 도달하면 0을 반환하고, 그 값은 `length(struct Node* head)`이 된다. 따라서 `1 + length(struct Node* head)`은 0 + 1이 되어 1이 된다. 이러한 방법으로 Node의 개수를 구하면 된다.

```c
void length(struct Node* head) {
    if(head == NULL) return 0;
    return 1 + length(head -> next);
}
```

**이 글에 잘못된 내용이 있을 경우 알려주시기 바랍니다.**