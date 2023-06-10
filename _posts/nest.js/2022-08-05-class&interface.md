---
layout: single
title: "[TypeScript] Class & Interface"
categories: NestJs
tag: [TypeScript]
toc: true
---

# 왜 공부를 하는가?

기술 면접에서 TypeScript의 Class와 Interface의 차이점이 무엇이냐는 질문과 덕타이핑에 대한 질문을 받았었다. 평소 functional 한 코드를 짰던 지라 class에 대해서 잘 몰랐었는데, 이번에 한 번 공부를 해보고자 한다.

# Class란?

Class는 TypeScript에만 있는 개념이 아니다. Java, python, C 등 많은 곳에서 이용된다. class 란 **object를 만드는 청사진 이다.** 이 청사진을 이용해 객체를 만드는 역할을 한다. 이 **청사진을 바탕으로 한 객체**를 인스턴스 라 부른다.

예를 들어, 내가 붕어빵 집 사장이라고 생각해보자. 나는 붕어빵을 만들기 위해, 붕어빵 틀을 가지고 있다. 이 붕어빵 틀로 붕어빵을 만들 수 있다.
붕어빵 틀에는 '반죽', '들어갈 재료' 등등이 들어 갈 수 있다. 붕어빵 틀에 반죽과 팥을 넣으면 '팥 붕어빵' 이 나오고, 반죽과 슈크림을 넣으면 '슈크림 붕어빵'이 나오게 된다. 이 객체를 **인스턴스**라고 보면 된다. 그리고 붕어빵 틀을 **class**로 볼 수 있다.

> 클래스는 속성(property) 와 메서드(method)를 갖는다.

## Class 예시

```ts
class Bread {
  // 틀
  dough: string; // property (안에 들어갈 것들)
  ingrement: string;

  constructor(dough: string, ingrement: string) {
    // 생성하기 위한 메소드
    this.dough = dough;
    this.ingrement = ingrement;
  }
}
```

위에 예시 처럼 붕어빵 틀을 Bread로 만들어 보았다. 이 틀에는 dough(반죽), 재료(ingrement)가 들어가야 붕어빵을 만들 수 있다. 이 틀에는 어떠한 것들이 들어가야 해요 라는 것을 명시한 것이다.

> class = 틀, property = 안에 들어가야 할 것들

여기서 contructor이라는 메소드가 보이는데, 이것은 실질적으로 이 **클래스를 만들기 위한 생성자 메소드** 이다.

이렇게 클래스를 선언 했지만, 실제로 만들어 진 것은 아니다. 그래서 우리는 contructor를 이용하여 class의 객체를 만들어야 한다. 이때 우리는 constructor이라는 메소드를 직접적으로 사용하는 것이 아니라 **new**라는 키워드를 사용하여 만들 수 있다.

```ts
class Bread {
  // 틀
  dough: string; // property (안에 들어갈 것들)
  ingrement: string;

  constructor(dough: string, ingrement: string) {
    // 생성하기 위한 메소드
    this.dough = dough;
    this.ingrement = ingrement;
  }
}

const redBeanBread = new Bread("dough", "redBean");
const creamBread = new Bread("dough", "cream");
```

이렇게 new라는 키워드를 사용하여 class의 객체를 사용할 수 있다. 이때 Bread의 property가 모두 있어야 생성이 된다. 생성을 해서 찍어보면 이렇게 객체가 생성되는 것을 볼 수 있다.

```
Bread { dough: 'dough', ingrement: 'redBean' }
Bread { dough: 'dough', ingrement: 'cream' }
```

이번엔 argument가 하나 들어간 클래스를 만들어 보자.

```ts
const wrong = new Bread("dough");
```

콘솔을 찍어보면 이렇게 나온다.

<img src="/assets/images/class.png">

이렇게 property에는 2개가 있어야 하는데, argument에는 하나 밖에 없다는 에러가 뜬다.

> JavaScript에서는 argument에 하나만 넣어 줘도 정상적으로 실행이 되고, 없는 property는 undefined로 나온다.

이번엔 interface를 알아보자.

# interface 란?

interface는 일반적으로 **타입 체크를 위해 사용되며 변수, 함수, 클래스에 사용**할 수 있다. interface는 여러가지 타입을 갖는 property로 이루어진 새로운 타입을 정의하는 것과 같다.

즉, 인터페이스에 선언된 **프로퍼티 또는 메소드의 구현을 강제하여 일관성을 유지할 수 있도록**하는 것이다.

class와 interface의 차이를 알아보기 위해 예시부터 확인해보자.

## interface의 예시

인터페이스는 다음과 같이 선언을 할 수 있다.

```ts
interface User {
  id: number;
  name: string;
  job: string;
}
```

이렇게 만들어 볼 수 있는데, class와 마찬가지로 첫 문자를 대문자로 한다.
User라는 인터페이스에는 프로퍼티로, number 타입의 id, string 타입의 name, string 타입의 job을 갖는다 라는 것과 같다.

하지만 이렇게 선언만 하면 class와 마찬가지로 인터페이스는 쓸 수가 없다.

```ts
let user: User;
```

이런 식으로 어떠한 변수에 타입을 설정 해주듯이 해줘야 한다. 지금은 user라는 변수에 인터페이스인 User를 상속 시켜 주었다.

```ts
let user: User;

user = { id: 1, name: "jihun", job: "null" };
```

이렇게 user라는 변수에 User 인터페이스에 있는 프로퍼티들을 넣어 다시 선언 해주었다. 콘솔로 찍어보면 이렇게 나온다.

```ts
{ id: 1, name: 'jihun', job: 'null' }
```

즉, 인터페이스는 class 처럼 프로퍼티와 메소드를 가질 수 있으나, **직접 인스턴스를 생성할 수 없다.**

그렇다면 인터페이스에 있는 프로퍼티를 하나 빼고 user1을 만들어 보면 어떻게 될까?

```ts
let user1: User = { id: 2, name: "John" };

console.log(user1);
```

<img src="/assets/images/interface.png">

아까 class와 마찬가지로 프로퍼티가 하나 빼먹었다고 에러가 나온다.

# class와 interface의 차이점

클래스와 interface의 예시를 통해 알아보았다. 그렇다면 가장 큰 차이점은 무엇인가?

> 클래스는 직접 인스턴스를 생성할 수 있으나, 인터페이스는 직접 인스턴스를 만들 수 없다.

# 마치며

오늘은 면접 질문에 나온 것들을 알아보았다. TypeScript를 조금 더 깊이있게 공부를 해야 겠다는 생각이 들었다.
