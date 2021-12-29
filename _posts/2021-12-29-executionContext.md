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
- 자동으로 생성되는 전역공간, eval를 제외하면 우리가 흔히 실행 컨텍스트를 구성하는 방법으 **함수를 실행**하는 것 뿐이다.

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

<!-- # ValiableEnvironment

- 여기에 담기는 내용은 LexicalEnvironment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다르다.
- 실행 컨텍스트를 생성할 때 ValiableEnvironment에 정보를 담은 다음, 이를 그대로 복사해서 LexicalEnvironment를 만들고, 이후에는 LexicalEnvironment를 주로 활용하게 된다.
- ValiableEnvionment와 LexicalEnvironment에는 environmentRecord와 outerEnvironmentReference로 구성돼있다.

# LexicalEnvironment

- 사전적 정의로는 '어휘적 환경', '정적 환경'이라고 한다.
- 현재 컨텍스트의 내부에는 a, b, c와 같은 식별자들이 있고 그 외부 정보는 D를 참조하도록 구성 돼있다 와 같이, 컨텍스트를 구성하는 환경 정보들을 사전에서 접하는 느낌으로 모아놓은 것을 의미한다.

## environmentRecord와 호이스팅

- environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다. 컨텍스트 내부 전체를 처음부터 끝까지 쭉 훑어나가며 **순서대로** 수집한다.
- 변수 정보를 수집하는 과정을 모두 마쳤더라도 아직 실행 컨텍스트가 관여할 코드들은 실행되기 전의 상태이다.
- 자바스크립트 엔진은 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행한다 라고 생각해도 문제 없다. -->
