---
layout: single
title: "SEB Section 2 Component Design"
categories: [codestates]
tag: [React]
toc: true
---

기획자로부터 하나의 페이지 기획이 도착했고, 페이지를 완성 시켰다. 그런데 다른 페이지에 적용되는 버튼에 대한 추가적인 기획안이 도착했다. 그런데 버튼의 기획안이 **이전에 요청받았던 버튼을 똑같이 사용하도록 요청**이었다.

이럴경우 처음부터 다시 페이지를 제작 해야 할까? 만약 여러 팀 간에 **같은 UI 컴포넌트**를 공유한다면 고민을 하지 않아도 될 것이다. 따라서 **재사용 할 수 있는 UI컴포넌트**를 미리 디자인 하고 개발 하는 것이 중요하다.

# Component Driven Developement(CDD)

이 고민을 해결하기 위한 방법이 CDD이다. **부품 단위로 UI컴포넌트를 만들어 나가는 개발**을 위한 방법이다.

## StoryBook

UI 개발도구 중 하나로 각각의 컴포넌트를 따로 볼 수 있게 구성해 주어 한 번에 하나의 컴포넌트에서 작업할 수 있도록 도와준다. 컴포넌트를 문서화하고, 자동으로 컴포넌트를 시각화하여 시뮬레이션할 수 있는 다양한 테스트 상태를 확인할 수 있다.

주요 기능

- UI 컴포넌트들을 카탈로그와 하기
- 컴포넌트 변화를 Stories로 저장하기
- 핫 모듈 재 로딩과 같은 개발 툴 경험을 제공하기
- 리액트를 포함한 다양한 뷰 레이어 지원하기

### 설치 및 세팅방법

```
# Clone the template
npx degit chromaui/intro-storybook-react-template taskbox

cd taskbox

# Install dependencies
yarn
```

# CSS 구조화

컴포넌트 뿐만 아니라 CSS도 구조화 하는 개발 방법에 대해서도 많은 연구가 이루어졌다. 여러 방법들이 생겼지만 각각의 방법론 모두 같은 지향점을 갖고 있다.

- 코드의 재사용
- 코드의 간결화(유지 보수 용이)
- 코드의 확장성
- 코드의 예측성(클래스 명으로 의미 예측)

CSS는 컴포넌트 기반의 방식으로 만들어진 적이 한 번도 없었다. 하지만 컴포넌트 영역으로 불러들이기 위해 CSS-in-JS가 탄생함으로써 문제를 해결하게 된다.

대표적으로 Styled-Component가 있는데, 이는 기능적 혹은 상태를 가진 컴포넌트들로부터 UI를 완전히 분리해 사용할 수 있는 패턴을 제공한다.

# Styled-CSS

styled-CSS를 작성하는 방식은 JS에서 변수를 선언하듯이, 만들고자 하는 변수명을 만들고 tag의 속성을 정의한다. 백틱(``) 안에 기존 CSS 문법을 이용하여 스타일 속성을 정의해주면 된다.

```js
const Button = styled.a`
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;
  width: 11rem;
`;
```

## Styled Component 특징

- Automatic critical CSS

화면에 어떤 컴포넌트가 렌더링 되었는지 추적해서 해당하는 컴포넌트에 대한 스타일을 자동으로 삽입한다.

- No class name bugs

스스로 유니크한 className을 생성한다. 중복이나 오타로 인한 버그를 줄여준다.

- Easier deletion of CSS

컴포넌트를 사용하지 않아 삭제할 경우 스타일 속성도 함께 삭제된다.

- Simple dynamic styling

props 나 전역 속성을 기반으로 컴포넌트에 스타일 속성을 부여하기 때문에 간단하고 직관적이다.

- Painless maintenance

컴포넌트에 스타일을 상속하는 속성을 찾아 다른 CSS 파일들을 검색하지 않아도 되기 때문에 코드가 커지더라도 유지 보수가 어렵지 않다.

- Automatic vendor prefixing

개별 컴포넌트마다 기존의 CSS를 이용하여 스타일 속성을 정의하면 된다.

### 설치 방법

```
# with npm
$ npm install --save styled-components

# with yarn
$ yarn add styled-components

# package.json
{
  "resolutions": {
    "styled-components": "^5"
  }
}
```
