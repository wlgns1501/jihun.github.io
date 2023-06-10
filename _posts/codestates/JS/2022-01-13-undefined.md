---
layout: single
title: "SEB Section 1 undefiend & null"
categories: codestates-Js
tag: [JS]
toc: true
---

# Undefinded

- '없음'을 나타내는 값이다.
- 사용자가 명시적으로 지정할 수 있다.
- 값이 없을 때, 자바스크립트 엔진이 자동으로 부여할 수 있다.

- 자바스크립트 엔진이 자동으로 undefined를 부여하는 경우

  1. 값을 대입하지 않은 변수(데이터 영역에 메모리 주소를 지정하지 않은 식별자)
  2. 객체 내부의 존재하지 않는 프로퍼티를 접근하려고 할 때
  3. return 문이 없거나 호출되지 않는 함수의 실행 결과

- 배열의 경우 조금 특이하다.

```js
var arr1 = [];
arr1.length = 3;
console.log(arr1); // [empty x  3]

var arr2 = new Array(3);
console.log(arr2); // [empty x  3]

var arr3 = [undefined, undefined, undefined];
console.log(arr3); // [undefined, undefined, undefined]
```

- 빈 배열을 만들고, n개의 빈 요소를 확보를 했지만 undefined로 할당이 되지 않는다.
- 이는 '비어 있는 요소'와 'undefined를 할당한 요소'와의 출력하는 결과가 다르다는 것이다.
- 또한 forEach, map, filter, reduce 등 배열의 각 요소를 순회하며 추가적인 기능을 하는 메서드 들에서 서로 다른
  결과를 보인다.
- 배열도 객체이므로, 값을 지정되지 않은 인덱스들은 '존재하지 않는 프로퍼티'에 지나지 않는다.

# Null

- '비어있음'을 명시적으로 나타내고 싶을 때 쓴다.
