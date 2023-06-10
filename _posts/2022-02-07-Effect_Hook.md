---
layout: single
title: "SEB Section 2 React Data"
categories: [codestates]
tag: [React]
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

그래서 react에서는 이러한 side Effect를 관리하기 위해서 Effect Hook이라는 것을 만들었다.
useEffect를 이용하여 순수 함수를 추구하는 react는 side Effect를 관리한다.
useEffect의 첫 번째 인자는 함수로 세 가지 조건에서 실행이 된다.

- 컴포넌트 생성 후 처음 화면에 렌더링
- 컴포넌트에 새로운 props가 전달되며 렌더링
- 컴포넌트에 상태가 바뀌며 렌더링

이와 같이 매번 새롭게 컴포넌트가 **렌더링**될 때 Effect Hook이 실행 된다.

## Hook을 쓸 때 주의점

- 최상위에서만 Hook을 호출한다.
- React 함수 내에서 Hook을 호출한다.

## 조건부 effect 발생

useEffect의 두 번째 인자는 배열이다. 이 배열은 조건을 가지고 있는데, boolean이 아닌 **어떤 값의 변경이 일어날 때**를 의미한다. 해달 배열엔 **어떤 값**의 목록이 들어가게 되는데, 이 배열을 종소성 배열이라고 부른다.

### 단 한 번만 실행 되는 Effect 함수

종속성 목록에 아무런 종속성이 없다면, 즉 빈 배열이 들어간다면 어떻게 될까?
이때는 **컴포넌트가 처음 생성될 때만** effect 함수가 실행된다. 외부 API를 통해 리소스를 받아오고, 더 이상 API호출이 필요하지 않을 때 사용할 수 있다.
