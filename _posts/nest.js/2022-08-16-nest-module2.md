---
layout: single
title: "[Side-Project] Nest Dynamic Module"
categories: NestJs
tag: [JavaScript, Side-Project]
toc: true
---

# 동적 모듈

동적 모듈은 모듈이 생성될 때 동적으로 어떠한 변수들이 정해진다. 즉, 호스트모듈(프로바이더나 컨트롤러와 같은 컴포넌트를 제공하는 모듈)을 가져다 쓰는 소비모듈에서 호스트모듈을 생성할 때 동적으로 값을 설정하는 방식이다.

대표적인 예로는 Config라고 부르는 모듈이 있다. 실행환경에 따라 서버에 설정되는 환경변수를 관리하기 때문에 동적모듈이라고 볼 수 있다.

# Config 패키지

Node.js에서 Config를 설정해주기 위해서, 타입스크립트에서 적용할 수 있는 @types/config 패키지를 다운받아 사용했지만, nest에서는 **@nestjs/config 패키지**를 다운받아 사용한다.

이 패키지에서는 ConfigModule 모듈이 이미 존재한다. 이 모듈을 동적 모듈로 가져와서 사용한다.

```ts
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule.forRoot()],
  ...
})
export class AppModule { }
```

정적 모듈을 가져올 때와는 달리 ConfigModule.forRoot() 메소드를 호출하는 것을 볼 수 있다. forRoot() 메소드는 동적모듈을 리턴하는 정적 메소드 이다.

```ts
export declare class ConfigModule {

    static forRoot(options?: ConfigModuleOptions): DynamicModule;  // 동적 모듈 리턴 해준다.
```

이 메소드의 인자로 Options를 줄 수 있는데, envFilePath를 통해 .env 파일을 읽어줄 수 있다.

```ts
import { ConfigModule } from "@nestjs/config";

@Module({
  imports: [
    ConfigModule.forRoot({
      envFilePath:
        process.env.NODE_ENV === "production"
          ? ".production.env"
          : process.env.NODE_ENV === "stage"
          ? ".stage.env"
          : ".development.env",
    }),
  ],
  controllers: [AppController],
  providers: [AppService, ConfigService],
})
export class AppModule {}
```

> Nest 공식문서에는 ConfigModule을 동적 모듈로 직접 작성하는 예가 나와 있다. @nest/config 패키지를 사용하지 않고 직접 dotenv를 사용하여 .env 파일이 존재하는 folder를 동적으로 전달하도록 한다.

- [Nest 공식문서 config 동적 모듈]('https://docs.nestjs.com/modules')
