---
layout: single
title: "SEB Section 2 Combinations & Permutaions"
categories: [codestates]
tag: [Algorithm]
toc: true
---

오늘 Toy 문제를 풀다보니, 첫 문제부터 순열을 사용해야 하는 문제가 나왔다.
본격적으로 알고리즘의 문제가 시작 된것이다. 순열과 조합은 세트로 묶어서 이해를 하는 것이 편할 것 같아서 같이 포스팅을 해본다.

# 조합

먼저 조합에 대해서 알아보자. 조합을 먼저 알아보는 이유는 코드로 구현할 때 조합의 코드에서 한 줄만 변경을 하면 순열을 구하는 코드로 작성할 수 있기 때문이다.
조합은 **순서와 상관 없이**, 나올 수 있는 모든 조합을 구한다는 말이다.

```
Input : [1, 2, 3, 4]
Output : [ [1,2,3], [1,2,4], [1,3,4], [2,3,4] ]
```

4개 중 3개를 선택하여 나올 수 있는 조합의 모든 수를 구하는 것이다. 이 때, 조합의 순서는 상관 안한다. 즉 [1,2,3] = [3,2,1] 이렇게 순서가 바뀌어도 같은 구성이기 때문에 하나의 조합으로 취급한다.

구하는 방법은 하나를 선택하여 고정으로 넣어 놓고, 나머지의 요소에서 어떻게 조합을 짤 수 있는지를 구하는게 포인트이다.

```
- 1을 선택(고정)하고 -> 나머지 [2, 3, 4] 중에서 2개씩 조합을 구한다.
  [1, 2, 3] [1, 2, 4] [1, 3, 4]
- 2를 선택(고정)하고 -> 나머지 [3, 4] 중에서 2개씩 조합을 구한다.
  [2, 3, 4]
- 3을 선택(고정)하고 -> 나머지 [4] 중에서 2개씩 조합을 구한다.
  []
- 4을 선택(고정)하고 -> 나머지 [] 중에서 2개씩 조합을 구한다.
  []
```

- 고정 값 구하기

  경우의 수를 구할 때 첫 번째 자리에 들어올 수 있는 경우의 수, 즉 1,2,3,4가 들어 올 수 있다.

- 고정 값과 나머지 숫자들의 조합 구하기

  순차적으로 1과의 조합을 구하는데 1은 이미 처음에 선택을 했고, 중복이 불가능 하니 나머지 2, 3, 4 중에서 2개를 선택하여 1과의 조합을 구한다.

- 순차적으로 고정 값을 변경하여 나머지 숫자들과 조합을 구하기

  고정값 1의 조합을 모두 구했으니, 고정값을 2로 변경을 하여 나머지 숫자 즉, 3, 4와의 조합을 구한다. 나머지 숫자에 1이 포함이 되지 않는 이유는 조합은 **순서를 상관하지 않기**때문에 넣지 않는다.

이런 식으로 조합을 구하게 된다. 이를 코드로 구현을 하려면 어떻게 해야 할까?

## 구현

앞서서 조합을 구하는 방식을 살펴 봤을 때, 고정값을 선택을 하고 나머지 숫자들과 조합을 구하는 모습이 반복되는 모습을 보았다. 따라서 이럴때는 **재귀 함수**를 사용하는 것이 좋다.

- 재귀 종료 조건 : 하나를 선택할 때에는, 모든 배열의 원소를 선택해서 리턴한다.
- forEach문으로, 배열의 모든 원소를 처음부터 끝까지 한 번씩 고정이 되도록 한다.
- 조합을 만들어온 결과에 고정된 값을 앞에 붙여서 리턴할 배열에 spread syntax로 배열화 한 후 리턴한다.

```js
const getCombinations = function (arr, selectNumber) {
  const results = [];
  if (selectNumber === 1) return arr.map((value) => [value]); // 1개씩 택할 때, 바로 모든 배열의 원소 return

  arr.forEach((fixed, index, origin) => {
    const rest = origin.slice(index + 1); // 해당하는 fixed를 제외한 나머지 뒤
    const combinations = getCombinations(rest, selectNumber - 1); // 나머지에 대해서 조합을 구한다.
    const attached = combinations.map((combination) => [fixed, ...combination]); //  돌아온 조합에 떼 놓은(fixed) 값 붙이기
    results.push(...attached); // 배열 spread syntax 로 모두다 push
  });

  return results; // 결과 담긴 results return
};

const example = [1, 2, 3, 4];
const result = getCombinations(example, 3);
console.log(result);
// => [ [ 1, 2, 3 ], [ 1, 2, 4 ], [ 1, 3, 4 ], [ 2, 3, 4 ] ]
```

# 순열

순열은 조합과 달리 **순서가 중요**하다. 실젤 순열을 구하는 공식도 조합으로부터 도출 가능하다.

```
nPr = nCr * r!
조합을 구한 후, 선택하려는 수의 factorial 을 곱하면 순열을 구하는 공식이 탄생한다.
```

조합을 구하는 코드와 별반 다르지 않지만, 조합을 구하는 코드에서 순열을 구하는 재귀함수 로직을 설정하면 된다.

## 구현

```js
const getPermutations = function (arr, selectNumber) {
  const results = [];
  if (selectNumber === 1) return arr.map((value) => [value]); // 1개씩 택할 때, 바로 모든 배열의 원소 return

  arr.forEach((fixed, index, origin) => {
    const rest = [...origin.slice(0, index), ...origin.slice(index + 1)]; // 해당하는 fixed를 제외한 나머지 배열
    const permutations = getPermutations(rest, selectNumber - 1); // 나머지에 대해 순열을 구한다.
    const attached = permutations.map((permutation) => [fixed, ...permutation]); // 돌아온 순열에 대해 떼 놓은(fixed) 값 붙이기
    results.push(...attached); // 배열 spread syntax 로 모두다 push
  });

  return results; // 결과 담긴 results return
};

const example = [1, 2, 3, 4];
const result = getPermutations(example, 3);
console.log(result);
// => [
//   [ 1, 2, 3 ], [ 1, 2, 4 ],
//   [ 1, 3, 2 ], [ 1, 3, 4 ],
//   [ 1, 4, 2 ], [ 1, 4, 3 ],
//   [ 2, 1, 3 ], [ 2, 1, 4 ],
//   [ 2, 3, 1 ], [ 2, 3, 4 ],
//   [ 2, 4, 1 ], [ 2, 4, 3 ],
//   [ 3, 1, 2 ], [ 3, 1, 4 ],
//   [ 3, 2, 1 ], [ 3, 2, 4 ],
//   [ 3, 4, 1 ], [ 3, 4, 2 ],
//   [ 4, 1, 2 ], [ 4, 1, 3 ],
//   [ 4, 2, 1 ], [ 4, 2, 3 ],
//   [ 4, 3, 1 ], [ 4, 3, 2 ]
// ]
```
