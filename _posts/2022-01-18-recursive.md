---
layout: single
title: "SEB Section 2 Recursive"
categories: [JS/Node]
tag: [codestates, JS/Node]
toc: true
---

# Recursion

- Recursion은 동일한 구조의 더 작은 문제를 해결함으로써 주어진 문제를 해결하는 방법인 **재귀**라고 한다.
- 자기 자신을 호출하는 방식을 **재귀 호출**이라 부른다.

# 재귀는 언제 사용하는게 좋을까?

1. 주어진 문제르 비슷한 구조의 더 작은 문제로 나눌 수 있는 경우
2. 중첩된 반복문이 많거나 반복문의 중첩 횟수를 예측하기 어려운 경우

- 하노이의 탑, 조합 문제 등에 쓰인다.

# Factorial

```js
function fac(n) {
  if (n === 1) {
    return 1;
  }

  return n * fac(n - 1);
}
```

# fibonacci

```js
function fib(n) {
  if (n <= 1) return 1;

  return fib(n - 1) + fib(n - 2);
}
```
