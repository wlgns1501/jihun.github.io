---
layout: single
title: "SEB Section 1 Execution Context"
categories: [JS/Node]
tag: [codestates, JS/Node]
toc: true
---

# 실행 컨텍스트란?

- 실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다.

<img src="/assets/images/stack.png">

- 동일한 환경에 있는 코드들을 실행할 때, 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 콜 스택에 쌓아 올렸다가, 가장 위에 놓여있는 컨텍스트와 관련 있는 코드들을 실행하는 식으로 전체 코드의 환경과 순서를 보장한다.
- 여기서 '동일한 환경', 즉 하나의 실행 컨텍스트를 구성할 수 있는 방법으로 전역공간, eval()함수, 함수 등이 있다.
- 자동으로 생성되는 전역공간, eval를 제외하면 우리가 흔히 실행 컨텍스트를 구성하는 방법은 **함수를 실행**하는 것 뿐이다.

```js
// ----------------------------- (1)
var a = 1;
function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }
  inner(); // ------------------ (2)
  console.log(a); // 1
}

outer(); // ---------------------(3)
console.log(a); // 1
```

- 처음 코드를 실행하는 순간 (1) 전역 컨텍스트가 콜 스택에 담긴다.
  - 전역 컨텍스트는 자바스크립트 파일이 열리는 순간 전역 컨텍스트가 활성화 된다고 이해하면 된다.
- 전역 컨텍스트와 관련된 코드들을 순차적으로 진행을 하다가 (3)에서 outer 함수를 호출 하면 자바스크립트 엔진은 outer에 대한 환경 정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜스택에 담는다.
- 콜 스택에 전역 컨텍스트 위에 outer 실행 컨텍스트와 관련된 코드가 놓이게 된다.
- 또 outer 함수 내부에 있는 (2)를 만나게 되면, inner 실행 컨텍스트가 콜 스택에 outer 컨텍스트 위에 놓이게 될 것이다.

<img src="/assets/images/callstack.png">

- 따라서 콜 스택 최상단에 있는 inner 함수가 실행이 되고 콜 스택에서 지워지게 된다. 다음 최상단인 outer 함수가 실행이 되고 제거가 되고, 마지막으로 남은 전역 컨텍스트가 실행이 되면서 콜 스택에는 아무것도 남지 않게 되고 종료가 될것이다.
- 실행 컨텍스트를 콜 스택에 넣을 때, 환경 정보를 수집을 해서 실행 컨텍스트 객체에 넣어 저장을 한다고 했는데, 이때 이 객체에 이런 정보가 담긴다.
  - VariableEnvironment: 현재 컨텍스트 내의 식별자드에 대한 정보 + 외부 환경 정보, 선언 시점의 LexicalEnvironment의 스냅샷으로, 변경 사항은 반영 되지 않음.
  - LexicalEnvironment: 처음에는 VariableEnvironment와 같지만 변경 사항이 실시간으로 반영됨.
  - ThisBinding: this 식별자가 바라봐야 할 대상 객체.

# ValiableEnvironment

- 여기에 담기는 내용은 LexicalEnvironment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다르다.
- 실행 컨텍스트를 생성할 때 ValiableEnvironment에 정보를 담은 다음, 이를 그대로 복사해서 LexicalEnvironment를 만들고, 이후에는 LexicalEnvironment를 주로 활용하게 된다.
- ValiableEnvionment와 LexicalEnvironment에는 environmentRecord와 outerEnvironmentReference로 구성돼있다.

# LexicalEnvironment

- 사전적 정의로는 '어휘적 환경', '정적 환경'이라고 한다.
- 현재 컨텍스트의 내부에는 a, b, c와 같은 식별자들이 있고 그 외부 정보는 D를 참조하도록 구성 돼있다 와 같이, 컨텍스트를 구성하는 환경 정보들을 사전에서 접하는 느낌으로 모아놓은 것을 의미한다.

## environmentRecord와 호이스팅

- environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다. 컨텍스트 내부 전체를 처음부터 끝까지 쭉 훑어나가며 **순서대로** 수집한다.
- 변수 정보를 수집하는 과정을 모두 마쳤더라도 아직 실행 컨텍스트가 관여할 코드들은 실행되기 전의 상태이다.
- 자바스크립트 엔진은 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행한다 라고 생각해도 문제 없다.
- 결국 식별자를 최상단으로 끌어올려놓는다는 것은 '호이스팅'을 의미한다.

### 호이스팅 규칙

- environmentRecord에서는 매개변수의 이름, 함수 선언, 변수명이 담긴다.

(원본 코드)

```js
function a(x) {
  // 수집대상 1(매개변수)
  console.log(x); //  1
  var x; // 수집대상 2(변수 선언)
  console.log(x); // undefined
  var x = 2; // 수집대상 3(변수 선언)
  console.log(x); // 2
}
a(1);
```

- 매개변수에 인자를 담는 것을 제외를 하면 내부에 변수 선언한 것과 다른점이 없다.
- 따라서 코드내부에서 매개변수를 먼저 변수에 선언을 하면 다를 것이 없다.
- LexicalEnvironment 입장에서는 완전히 같다.
- 그래서 이렇게 바꾸어 보겠다.

(매개변수를 변수 선언/할당과 같다고 간주해서 변환된 상태)

```js
function a() {
  var x = 1; //수집대상 1(매개변수 선언)
  console.log(x); //  1
  var x; // 수집대상 2(변수 선언)
  console.log(x); // 1
  var x = 2; // 수집대상 3(변수 선언)
  console.log(x); // 2
}
a();
```

- envrionmentRecord는 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지에만 관심이 있다.
- 각 식별자에 어떤 값이 할당될 것인지는 관심이 없다.
- 따라서 호이스팅이 일어나게 되면, 변수명만 끌어 올리고, 할당 과정은 남겨둔다. 매개변수 경우도 마찬가지다.

(호이스팅 마친 상태)

```js
function a() {
  var x; // 수집대상 1의 변수 선언 부분
  var x; // 수집대상 2의 변수 선언 부분
  var x; // 수집대상 3의 변수 선언 부분

  x = 1; //수집대상 1의 할당 부분
  console.log(x); //  1
  console.log(x); // 1
  x = 2; // 수집대상 3의 할당 부분
  console.log(x); // 2
}
a();
```

- 변수 선언은 위로 올려지고 할당부분은 남겨지므로, 호이스팅이 일어나면 출력 값이 변하게 된다.

## 스코프, 스코프 체인, outerEnvironmentReference

- 스코프란 식별자에 대한 유효 범위로, 전역 변수는 모든 범위 내에서 접근이 가능 하지만,
  코드 블록 안에 선언한 변수는 내부에서만 접근이 가능하다.
- '식별자의 유효범위'를 안에서부터 바깥으로 차례로 검색해나가는 것을 스코프 체인이라고
  한다.
- 이를 가능하게 하는 것이 바로 LexicalEnvironment의 수집자료인
  outerEnvironmentReference 이다.

### 스코프 체인

- outerEnvironmentReference는 현재 호출된 함수가 선언될 당시의
  LexicalEnvironment를 참조한다.
- 과거 시점인 '선언될 당시'에 주목을 해야 한다.
- 선언하다라는 행위가 실제로 일어날 수 있는 시점이란 콜 스택 상에서 어떤 실행 컨텍스트가
  활성화된 상태, 즉 실행될 때 이다.

- 함수 a 안에 함수 b를 선언하고, 함수 b 안에 함수 c를 선언을 한다면,
  c -> b -> a 순으로 각 내부의 함수 outerEnvironmentReference는 바로 접하는 외부함수의 LexicalEnvironment를 참조한다.
- 결국 여러 스코프에서 동일한 식별자를 선언한 경우에는
  **무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능**하게 된다.
