---
title: "2018 KAKAO BLIND RECRUITMENT_비밀 지도"
toc: true
toc_label: "Table of Contents"
categories:
  - Algorithm
tags:
  - Programmers.co.kr
---

# 2018 KAKAO BLIND RECRUITMENT_비밀 지도

[문제 바로가기](https://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1)

## 요지

이 문제의 요지는

1. '겹치다'의 뜻을 이해하기
2. 1의 결론을 바탕으로 `arr1`,  `arr2` 처리하기
   1. 각 지도(`vector`)의 원소의 이진 값을 배열처럼 접근하는 방법
   2. 1의 방법을 이용해 `answer` 만들기

이다.

## 해결

### 요지 1

출력은 `arr1` 의 어느 한 원소와 `arr2` 의 같은 위치의 값을 '겹쳐' 만들어진 `answer` 를 반환하면 된다.

'겹치다' 라는 단어는 문제의 설명 2번에 나온다. 설명에 따르면 지도 1(또는 `arr1`), 지도 2(또는 `arr2`)중 어느 하나라도 벽(또는 1)인 부분은 전체 지도(또는 `answer`)에서도 벽이고, 지도 1, 2 에서 모두 공백인 부분은 전체 지도에서도 공백(또는 0)이 된다고 한다. 

| `arr1` | `arr2` | `answer` |
| ------ | ------ | -------- |
| 1      | 0      | 1        |
| 0      | 0      | 0        |

따라서 '겹치다'의 뜻은 OR 연산임을 알 수 있다.

### 요지 2 의 1

예를 들어 십진수 9는 이진수로 01001이다. 이를 배열처럼 접근하는 방법은 1과 각 자릿수를 &(`AND`)연산하면 된다. 연산 방법은 다음과 같다:

```cpp
9 & (1 << i) // i 는 각 자릿수를 의미한다
```

### 요지 2 의 2

`answer` 는 `string` 으로 이루어진 1차원 `vector` 이므로 `arr1[i]` 와 `arr2[i]` 의 이진 값의 자릿수를 OR 연산한 결과를 `answer[i]` 에 덧붙이면 된다. 따라서 코드는 다음과 같다:

```cpp
for(i = 0; i < n; i++) {
  string temp = "";
  for(j = n - 1; j >= 0; j--) {
    operand = (1 << j);
    temp += arr1[i] & operand || arr2[i] & operand ? '#' : ' ';
  }
  answer.push_back(temp);
}
```
