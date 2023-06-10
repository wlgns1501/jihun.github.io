---
layout: single
title: "[Side-Project] Nest Module"
categories: NestJs
tag: [JavaScript, Side-Project]
toc: true
---

# Module

Node.js로 개발을 할 때, 라우터 별로 각자의 할 일을 구분지어 로직을 짰었다. 예를 들어, user와 관련된 것은 User 라우터를 만들어, 그 밑에 Http 메소드 별로, 수행하는 함수를 만들어서 사용했었다.

Nest에서는 이를 모듈로 만들어서 하나의 일만 수행할 수 있도록 한다. 그렇다면 모듈은 어떻게 구성이 되어있을까?

<img src="/assets/images/module-1.png">

위의 그림처럼 모듈은 트리 구조(?)로 되어있다. 하나의 루트 모듈이 존재를 하고, 이 루트 모듈 안에 다른 모듈들로 구성이 되어있다. 이렇게 모듈로 쪼개는 이유로는 앞서 설명한 여러 모듈에게 각기 맡은 바 책임을 나누고 응집도를 높이기 위함이다. 어떻게 모듈을 나눌것인가 라는 명확한 기준은 없지만, 유사한 기능끼리 모듈로 묶어야 한다.

모듈은 @Module 데코레이터를 사용한다. 이 데코레이터는 인자로 ModuleMetadata를 받는다.

```ts
export interface ModuleMetadata {
  /**
   * Optional list of imported modules that export the providers which are
   * required in this module.
   */
  imports?: Array<
    Type<any> | DynamicModule | Promise<DynamicModule> | ForwardReference
  >;
  /**
   * Optional list of controllers defined in this module which have to be
   * instantiated.
   */
  controllers?: Type<any>[];
  /**
   * Optional list of providers that will be instantiated by the Nest injector
   * and that may be shared at least across this module.
   */
  providers?: Provider[];
  /**
   * Optional list of the subset of providers that are provided by this module
   * and should be available in other modules which import this module.
   */
  exports?: Array<
    | DynamicModule
    | Promise<DynamicModule>
    | string
    | symbol
    | Provider
    | ForwardReference
    | Abstract<any>
    | Function
  >;
}
```

이 모듈의 메타데이터의 인터페이스로는 이렇게 구성이 되어 있다.

- import : 이 모듈에서 사용하기 위한 프로바이더를 가지고 있는 다른 모듈을 가지고 온다.

- controller / providers : 모듈 전반에서 컨트롤러와 프로바이더를 사용할 수 있도록 Nest가 객체를 생성하고 주입할 수 있도록 한다.

- export : 다른 모듈에서 사용할 수 있도록 한다.

# 모듈의 종류

모듈은 두 가지로 구분할 수 있다.

- CommonModule : 서비스 전반에 쓰이는 공통 기능을 모아 놓은 모듈
- CoreModule : 공통 기능이기는 하지만 앱을 구동시키는 데 필요한 기능(로깅, 인터셉터 등) 모아둔 모듈

이때 루트 모듈인 AppModule은 앱을 구동시키기 위해 CoreModule이 필요한데 CommonModule의 기능도 필요하다. 이런 경우 둘 다를 가져오는 것이 아닌, CoreModule만을 가져오고 CoreModule에서는 가져온 CommonModule을 다시 내보내면 AppModule에서 CommonModule을 가져오지 않아도 사용할 수 있다.

```ts
// commonModule.ts

@Module({
  providers: [CommonService],
  exports: [CommonService],
})
export class CommonModule {}
```

CommoneModule에는 CommonService를 제공하고 있다.

```ts
// CommonService.ts

@Injectable()
export class CommonService {
  hello(): string {
    return "Hello from CommonService";
  }
}
```

CommonService는 hello라는 기능을 제공한다.

```ts
// AppModule.ts

@Module({
  imports: [CoreModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

AppModule은 CoreModule만 가져온다.

```ts
// AppController.ts

@Controller()
export class AppController {
  constructor(private readonly commonService: CommonService) {}

  @Get("/common-hello")
  getCommonHello(): string {
    return this.commonService.hello();
  }
}
```

이렇게 사용하면 AppModule에 속한 AppController에서 CommonModule에 기술된 CommonService 프로바이더를 사용할 수 있다.

> 모듈은 프로바이더처럼 주입해서 사용할 수 없다. 모듈간 순환 종속성이 발생하기 때문이다.

## 전역 모듈

Nest는 모듈 범위내에서 프로바이더를 캡슐화 한다. 따라서 어떤 모듈에 있는 프로바이더를 사용하려면 먼저 모듈을 가져와야 한다. 하지만 때에 따라서 전역적으로 쓸 수 있어야 하는 프로바이더가 필요한 경우가 있다. 이때 전역 모듈을 만들어 사용한다.

@Global 데코레이터를 사용하여 전역 모듈로써 사용할 수 있다. 하지만 전역 모듈을 만들어 사용하는건 응집도를 떨어뜨리기 때문에 좋지 않다. 따라서 꼭 필요한 기능만 모아 전역 모듈로 사용해야 한다.
