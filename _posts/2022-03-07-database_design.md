---
layout: single
title: "SEB Section 3 Database Design"
categories: [Node.js/Database]
tag: [codestates, Node/Database]
toc: true
---

# 관계형 데이터베이스

관계형 데이터베이스는 어떻게 설계를 해야 하는가?
구조화된 데이터는 하나의 테이블로 표현할 수 있다. 테이블을 사용하는 데이터베이스를 관계형 데이터베이스라고 한다. 다음의 키워드들을 알아보자.

- 데이터(data) : 각 항목에 저장되는 값이다.
- 테이블(table) : 사전에 정의된 열의 데이터 타입대로 작성된 데이터가 행으로 축적된다.
- 칼럼(column; field) : 테이블의 한 열을 가리킨다.
- 레코드(record) : 테이블의 한 행에 저장된 데이터이다.
- 키(key) : 테이블의 각 레코드를 구분할 수 있는 값이다. 각 레코드마다 고유한 값을 가진다. 기본키(primary key)와 외래키(foreign key) 등이 있다.

# 관계 종류

테이블과 테이블 사이의 관계는 다음과 같다.

- 1:1 관계
- 1:N 관계
- N:N 관계

테이블 스스로 관계를 가질 수도 있다.

- self referencing 관계

## 1:1 관계

하나의 레코드가 다른 테이블의 레코드 한 개와 연결된 경우이다.
