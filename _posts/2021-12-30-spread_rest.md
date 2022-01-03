---
layout: single
title: "SEB Section 1 Spread/rest"
categories: [JS/Node]
tag: [codestates, JS/Node]
toc: true
---

# Spread

- 전개 구문을 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 리터럴의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로 객체로 확장시킬 수 있다.
- spread를 사용하여 배열을 복사할 때, 본래의 배열은 변하지 않는다.(_immutable_)

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

## apply() 대체

- 일반적으로 배열의 엘리먼트를 함수의 인자로 사용할 때 apply를 사용했다.

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];
console.log(sum.apply(null, numbers)); // 6
```

- spread를 이용하여 대체를 할 수 있고, 여러번 사용될 수 있다.

## new에 적용

- new를 사용해 생성자를 호출 할 때, 배열과 apply를 직접 사용하는 것은 불가했다.
- 하지만 spread를 통해 배열을 new와 함께 쉽게 사용할 수 있다.

```js
var dateFields = [1970, 0, 1]; // 1 Jan 1970
var d = new Date(...dateFields);
```

# Rest

- 파라미터를 배열의 형태로 받아서 사용할 수 있다. 파라미터 개수가 가변적일 때 유용하다.

```js
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}

sum(1, 2, 3); // 6
sum(1, 2, 3, 4); // 10
```

- 함수에서 나머지 파라미터를 가져올 때 용이하다.

```js
function myFun(a, b, ...manyMoreArgs) {
  console.log("a", a); /// one
  console.log("b", b); /// two
  console.log("manyMoreArgs", manyMoreArgs); ///  ["three", "four", "five", "six"]
}

myFun("one", "two", "three", "four", "five", "six");
```

# 구조 분해

- 구조 분해 할당은 spread 문법을 이용하여 값을 해체한 후, 개별 값을 변수에 새로 할당하는 과정을 말한다.

(배열)

```js
const [a, b, ...rest] = [10, 20, 30, 40, 50]; /// a = 10, b = 20, rest = [30, 40, 50]
```

(객체)

```js
const { a, b, ...rest } = { a: 10, b: 20, c: 30, d: 40 }; // a = 10, b = 20, rest = {c: 30, d: 40}
```
