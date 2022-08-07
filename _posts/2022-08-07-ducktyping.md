---
layout: single
title: "[TypeScript] Duck Typing"
categories: [TypeScript]
tag: [TypeScript]
toc: true
---

# 왜 공부를 하는가?

이것 또한 면접 질문에 나와서 공부를 시작해본다. 생소한 단어를 물어봐서 배워놓아야 겠다는 생각이 들어 공부를 해본다.

# Duck Test

일단 먼저 Duck Typing이 무엇인가에 대해 알아보기 위해, 해당 용어를 위키백과에 찾아보았다. Duck Typing이란 Duck test에서 유래 되었다고 한다.

> 만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다.

어떤 새가 닭, 독수리, 부엉이라 할지라도 오리 처럼 걷고, 꽥꽥 소리 내고, 헤엄 치면 오리라고 부르겠다는 것이다.

# Duck Typing

다시 덕 타이핑으로 돌아와보자. 덕 타이핑은 동적 타이핑의 한 종류로, 객체의 변수 및 메소드의 집합이 객체의 타입을 결정하는 것을 말한다. 클래스 상속이나 인터페이스 구현으로 타입을 구분하는 대신, **덕 타이핑은 객체가 어떤 타입에 걸맞은 변수와 메소드를 지니면 객체를 해당 타입에 속하는 것으로 간주된다.**

> 동적 타이핑 : 런타임 시점에서 타입이 체크되어 지는 것 ex) Js
>
> 정적 타이핑 : 컴파일 시점에서 타입이 체크되어 지는 것 ex) Ts

타입스크립트는 정적 타이핑을 지원하는 언어이다. 그런데 앞서 덕 타이핑은 동적 타이핑의 한 종류라고 말했다. 이 둘의 성향이 맞지 않는데, 왜 타입스크립트에서는 덕 타이핑이 가능 한 것일까?

> 이 부분은 나에게서 이해하기 어려운 부분이라 나중에 시간 될 때 다시 다루도록 해보겠다.

## Duck Typing의 예시

```ts
interface IDuck {
  // 1
  quack(): void;
}

class MallardDuck implements IDuck {
  // 3
  quack() {
    console.log("Quack!");
  }
}

class RedheadDuck {
  // 4
  quack() {
    console.log("q~uack!");
  }
}

function makeNoise(duck: IDuck): void {
  // 2
  duck.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new RedheadDuck()); // q~uack! // 5
```

먼저 구조를 이해를 해보자.

- 인터페이스 IDuck은 quack 메소드를 정의하였다.

- makeNoise 함수는 인터페이스 IDuck을 구현한 클래스의 인스턴스 duck을 인자로 전달받는다.

- 클래스 MallardDuck은 인터페이스 IDuck을 구현하였다.

- 클래스 RedheadDuck은 인터페이스 IDuck을 구현하지는 않았지만 quack 메소드를 갖는다.

- makeNoise 함수에 인터페이스 IDuck을 구현하지 않은 클래스 RedheadDuck의 인스턴스를 인자로 전달하여도 에러 없이 처리된다.

여기서 만약 우리가 알고 있는 interface의 개념이라면, 두번째 makeNoise의 호출 함수는 에러가 떠야 할 것으로 생각이 들것이다. 하지만 에러가 나지 않고, RedheadDuck의 메소드가 정상적으로 실행이 되었다.

이는 TypeScript에서 **해당 인터페이스에 정의한 프로퍼티나 메소드를 가지고 있다면 그 인터페이스를 구현한 것으로 인정**하기 때문에 정상적으로 실행이 된다. 이것을 **덕 타이핑 또는 구조적 타이핑** 이라고 한다.

원래대로 라면, 컴파일 시점에서 RedheadDuck에 인터페이스를 가지고 있지 않아 TypeError가 떠야 한다.

# 마치며

JavaScript에서는 에러가 나더라도, 일단 실행을 하고 보지만,TypeScript를 사용 했을 때, 컴파일 시점에서 실행이 되기 전에 에러가 발생을 하면 실행이 되지 않아 에러를 판단 할 수 있게끔 해서 유용 하다 라는 느낌을 받았다. 하지만 이러한 부분에서 유연한 부분도 있구나, 덕 타이핑이 발생이 되었는데, 알지 못하게 된다면 골치 아플수도 있구나 라는 생각을 했다.
