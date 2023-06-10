---
layout: single
title: "SEB Section 1 qureySelector"
categories: [codestates]
tag: [HTML/CSS]
toc: true
---

# querySelector를 이용한 calculator 만들기

## 1. qureySelector 란?

- 각 tag들은 고유의 id나, class를 가지고 있다.
- 이 tag에 붙혀진 id 와 class를 선택하여, 문서 내 첫 번째 element를 반환 하는 것이 qureySeletor 이다.

## 2. calculator

- 계산기 화면, 숫자가 들어가는 태그의 id나 class를 가진 노드를 querySelector로 불러오고, 그 텍스트를 받아오기 위해 textContent를 사용했다.
- 이 textContent를 사용하여 자신이 누른 숫자와 연산 기호, 연산 결과값을 띄우기 위해 사용했다.

## 3. calculator를 만들면서 겪은 어려움

1. 첫 번째 계산기는 자릿수가 하나 인 숫자들을 계산하는 계산기 였다. 초기 화면은 0 + 0 = 0 이였다.

   ```javascript
   buttons.addEventListener("click", function (event) {
   // 버튼을 눌렀을 때 작동하는 함수입니다.

   const target = event.target; // 클릭된 HTML 엘리먼트의 정보가 저장되어 있습니다.
   const action = target.classList[0]; // 클릭된 HTML 엘리먼트에 클레스 정보를 가져옵니다.
   const buttonContent = target.textContent; // 클릭된 HTML 엘리먼트의 텍스트 정보를 가져옵니다.
   ```

   - 버튼을 눌렀을 때, 이벤트가 발생하는 버튼을 target으로 설정을 하고, class 정보와, text 정보를 가져왔다.
   - 숫자인 버튼을 눌렀을 때, 첫번째 숫자에 담겨 질 수 있도록 해야 하는데, 조건문을 세우지 않아서 두 번째 숫자에도 첫 번째 숫자에 들어간 숫자가 똑같이 들어갔다.

   ```javascript
   if (operator.textContent === "+" && firstOperend.textContent === "0") {
   ```

   - 그래서 이와 같이 연산 기호가 '+' 일 때, 즉 초기 화면 값일때, 그리고 첫 번째 숫자가 '0' 일 때 둘 다 만족을 할 경우에 첫 번째 숫자에 들어 갈 수 있도록 하였다.
   - 그랬더니 첫 번째 숫자와, 두 번째 숫자가 각각 나뉘어서 할당 되는 것을 볼 수 있었다.

## 4. calculator 를 만들고 나서

- 내가 생각한 것 들이 생각 한 대로 안되었을 때, 조금 짜증이 났다. 하지만 막상 다시 생각을 해보면 정말 세세한 오류 때문에 원하는 방향으로 안되어 지는 경우가 많았다. 앞으로는 조금 더 세밀하게 꼼꼼하게, 경우의 수를 따져가며 작성 하도록 노력해야겠다.
