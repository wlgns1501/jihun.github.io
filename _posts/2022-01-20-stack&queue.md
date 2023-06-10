---
layout: single
title: "SEB Section 2 Stack & Queue"
categories: [codestates]
tag: [Algorithm]
toc: true
---

# Stack

Stack은 쌓다, 쌓이다, 포개지다와 같은 뜻을 가지고 있다. 즉, **데이터를 순서대로 쌓는 자료구조**이다.
예를 들어, 앞이 막혀 있는 도로가 있다. 도로에 차가 들어오면서 앞으로 가야 하지만 막다른 길에 막혀 못가고 있는 상황이다.
뒤에 줄 지어서 차가 들어 오게 되고, 들어온 차들이 다시 이 도로를 빠져 나갈려면 맨 마지막으로 들어온 차가 나가야 나올 수 있을 것이다.

Stack을 정리를 해보자면

- 가장 마지막으로 들어온 데이터를 먼저 실행 시키는 것
- 후입선출법
- LIFO(Last In First Out)

이렇게 정리를 할 수 있겠다.

## 코드

```js
class Stack {
  constructor() {
    this._arr = [];
  }
  push(item) {
    this._arr.push(item);
  }
  pop() {
    return this._arr.pop();
  }
  peek() {
    return this._arr[this._arr.length - 1];
  }
}

const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
stack.pop(); // 3
```

# Queue

Queue는 줄을 서서 기다리다, 대기행렬 이라는 뜻을 가지고 있다. 큐는 스택과 반대되는 개념이다.

- 먼저 들어온 데이터가 먼저 실행된다.
- 선입선출법
- FIFO(First In First Out)

## 코드

```js
class Queue {
  constructor() {
    this._arr = [];
  }
  enqueue(item) {
    this._arr.push(item);
  }
  dequeue() {
    return this._arr.shift();
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.dequeue(); // 1
```
