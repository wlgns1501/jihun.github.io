---
layout: single
title: "[Side-Project] 왜 Nest.js를 이용하는가?"
categories: sideproject
tag: [JavaScript, Side-Project]
toc: true
---

# Nest.js 란?

Nest.js란 자바스크립트 모듈인 Node의 Express와 같은 서버 프레임워크중 하나이다. Nest.js의 공식 홈페이지에 보면 왜 Nest.js를 만들게 되었는지에 대해 나와있다. 한번 살펴보자.

## 탄생 배경

> 최근 몇 년 동안 Node.js 덕분에 JavaScript는 BE, FE 애플리케이션 모두 웹의 공통 언어가 되었습니다. 이로 인해 Angular, React, Vue가 나오게 되었으며, 해당 프로젝트를 통해 생산성을 향상하고 빠르게 만들 수 있으며, 테스트 가능하고 확장성이 있는 프런트엔드 애플리케이션을 만들 수 있게 되었습니다. 그러나 서버 측 Node.js에서는 뛰어난 라이브러리, 툴이 존재하지만 아키텍처의 주요 문제를 효과적으로 해결하는 것은 없었습니다.

> Nest는 개발자와 팀이 테스트 가능하고 확장이 가능하며, 느슨한 결합과 유지보수성이 뛰어난 애플리케이션을 만들 수 있도록 아키텍처를 제공합니다. 이 아키텍처는 Angular(느슨한 결합과 뛰어난 확장성을 가짐)에서 영감을 받았습니다.

이 글을 읽어보면 **Nest.js는 서버 측 어플리케이션 개발에 있어 아키텍처의 문제를 해결하기 위해 등장**한 것임을 알 수 있다.

Express를 사용은 매우 쉽고, 자유롭게 사용할 수 있다. 딱딱한 아키텍처의 구조가 없기 때문에 사람들 마다 아키텍처들이 다 다르다.

> 아키텍처 : 개발에 맞는 설계

그래서 프로젝트를 할 때, 나는 req, res를 받는 service 폴더 와 비즈니스 로직이 담겨 있는 db 폴더로 나누어 개발을 하는데, 다른 팀원은 또 다른 형식의 아키텍처로 개발한 경험이 있다. 이때 설명을 하고 논의를 하는 시간을 소비했던 경험이 있다.

이렇듯 Express는 웹 어플리케이션의 아키텍처에 대한 이야기는 없다. 그래서 Nest.js는 이러한 점을 보완 하고자 나오게 된 것이다.

## 장점

그렇다면 장점은 무엇일까?

- 확장에 용이하며 쉬운 서버 애플리케이션을 쉽게 개발할 수 있다.
- TypeScript 및 OOP (객체 지향 프로그래밍), FP(기능 프로그래밍), FRP(기능 반응성 프로그래밍) 요소를 결합한다.(효율성 증가)
- TypeScript를 사용함으로써 사전에 오류를 방지할 수 있다. 또한 모듈로 감싸는 형태로 개발하기 때문에 모듈 별로 테스트 코드를 쉽게 작성할 수 있도록 구현되어 있다.(안정적)
- Module를 통해 확장이 용이하도록 설계되어 있다.
- TypeScript를 사용하여 DI(Dependency Injection), IoC(Iversion of Control), 모듈을 통한 구조화 등의 기술을 통해 생산성이 높다.
- 데이터베이스, ORM, 설정, 유효성 검사 등 수많은 기능을 기본 제공 한다.

> DI : 의존성 주입
> IoC : 제어역전

# Nest.js 선정한 이유

Express를 사용하면서 자율성이 높지만, 팀 프로젝트에서 사용할 때 아키텍처에 대한 고민과 시간을 투자를 해야한다. 또한 내가 사용하고자 하는 기능을 수행하는 라이브러리를 찾아보고 설치를 해야 하는 불편함이 있었다.

Nest.js는 아키텍처에 대한 틀이 잡혀 있어, 팀 프로젝트 더 나아가 실무에서 이용하기에 용이할 것 같았고, 기본적인 내장 기능들이 들어 있어 선택을 하게 되었다.
