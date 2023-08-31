---
layout: single
title: "[weathervely 4] 온보딩 api 생성"
categories: sideproject
tag: [Side-Project, weathervely]
toc: true
---

# api 계획

이제 DB의 table을 fix 하였고, 온보딩에 대한 api를 제작하려고 한다.
온보딩의 과정은 닉네임 설정 -> 지역 설정 -> 성별 설정 -> 스타일 설정 -> 체감 온도 설정 으로 이루어져 있다.

닉네임의 글자수 제한은 10글자이고, 쉼표, \, 따옴표를 제외 한 모든 문자가 들어갈수 있도록 validation이 되어져야 한다.
또한 닉네임을 설정을 했을 때, 토큰을 만들어서 DB에 저장을 해야 한다.

만약 온보딩 과정을 마치지 않고, 도중에 앱을 종료 시켰을 경우 토큰을 통해 유저를 확인하고, 어느 과정에서 멈췄는지 알려줘야 한다.
그래야 그 부분에서 다시 온보딩 과정을 거칠 수 있기 때문이다.

지역 같은 경우 프론트 단에서 카카오 api를 통해 지역을 검색을 해서, 서버단으로 지역을 보내주게 된다. 이때 address 테이블에 있는지 확인을 한 후, 없으면 생성, 있으면 그대로 가져다가 조인 테이블에 저장을 하는 방식으로 해주기로 했다.

# setting

## swagger

먼저 api를 만들기 전에 swagger를 연결을 시켜줬다. 또한 Dto에서 편하게 apiProperty를 직접 만들어 주는 것이 아닌, entity단에서 각 컬럼마다 apiProperty를 해주었다.
이후 Dto에서 extends를 이용하여 가져오고 싶기 때문이다.

## exception filter

각 api를 만들 때 글로벌 예외 처리 필터가 필요하다. 기존에 nest에 있는 exception filter를 써도 좋지만, 조금 더 디테일 한 메세지가 들어갈 수 있게끔 해주고 싶었다.
그래서 exception filter와 business exception filter를 만들어 주었다.

### exception filter

이것은 exception filter implements 하는 custom exception filter이다. 여기서 에러를 캐치 하게 되는데, 에러를 바탕으로 reponse의 body를 생성해준다. 예외가 어떤 것인지 분기처리를 해주고, 예외를 response로 보내주게 된다.

### business exception filter

이 예외 필터 같은 경우, 조금 더 개발자들에게 에러를 쉽게 파악을 할 수 있도록 해준다. domain, 에러메세지, api message, status등을 파악 할 수 있다.

나중에 조금 더 상세하게 이 부분에 대해서 글을 써봐야 겠다.

# api 제작

api을 제작을 하려고 repository를 만들려고 하는데, typeorm의 버전을 0.3.x 버전을 쓰고 있어서, entityRepository 데코레이터가 삭제 되었다는 것을 발견하였다.

원래 0.2.x 버전을 썼었던 터라, 이 데코레이터가 삭제가 되어 다른 방법을 이용해 repository를 만들어 사용해야 한다.

어제 회의 때 service단에서 db 접근을 하는게 가시성도 안좋고 나중에 코드를 관리 할 때 힘들것 같아서, respository 단에서 db 접근을 수행하자고 말을 했는데 이 데코레이터가 삭제가 되어 많이 당황했다.

0.3.x 버전의 custom repository를 만들어 작업을 하는 것을 찾아보고 공부를 해야 겠다.
