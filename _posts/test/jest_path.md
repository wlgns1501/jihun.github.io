---
layout: single
title: "[Jest] 경로 설정"
categories: test
tag: [Jest, test]
toc: true
---


# Jest 경로 설정 에러

service 단의 test를 하려고 실행을 시켰더니, 경로 설정에 대한 에러가 발생하였다.

<img src='/assets/images/jest_path.png'>

이 에러는 package.json에 jest의 절대경로 설정을 해주지 않아서 발생하는 에러이다. 그래서 상대 경로를 설정을 해줘야 에러가 발생하지 않는다.

하지만 package.json의 jest 설정하는 부분에 

```js
"moduleNameMapper": {
      "^src/(.*)$": "<rootDir>/$1"
    }
```

이 부분을 작성을 해주면 절대 경로를 잘 가져오는 것을 볼 수 있다.