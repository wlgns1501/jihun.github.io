---
layout: single
title: "SEB Section 2 React Data"
categories: [React]
tag: [codestates, React]
toc: true
---

# React의 함수 컴포넌트

React의 함수 컴포넌트는 그 어떤 Side Effect가 없으며 순수 함수로 작동한다. 하지만 AJAX 요청, 다른 API의 사용은 Side Effect가 발생한다. 그래서 React는 Effect Hook을 제공한다.

## Side Effect (부수 효과)

함수 내에서 어떤 구현이 함수 외부에 영향을 끼치는 경우 해당 함수는 Side Effect가 있다고 이야기 한다.

```js
let foo = "hello";

function bar() {
  foo = "world";
}

bar(); // bar는 Side Effect를 발생시킵니다!
```

## Pure Function (순수 함수)

순수 함수란, 오직 함수의 입력만이 함수의 결과에 영향을 주는 함수를 의미한다. 함수의 입력이 아닌 다른 값이 함수의 결과에 영향을 미치는 경우, 순수 함수라고 부를 수 없다. 또한 입력으로 전달된 값을 수정하지 않는다.

```js
function upper(str) {
  return str.toUpperCase(); // toUpperCase 메소드는 원본을 수정하지 않습니다 (Immutable)
}

upper("hello"); // 'HELLO'
```

순수 함수에는 Side Effect가 없다. 순수 함수의 특징 중 하나는, 어떠한 전달 인자가 주어질 경우, 항상 똑같은 값이 리턴됨을 보장한다.

# Effect Hook
