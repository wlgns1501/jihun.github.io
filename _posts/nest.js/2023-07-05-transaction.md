---
layout: single
title: "[TypeOrm] Transaction"
categories: NestJs
tag: [Nest.js, typeorm, Side-Project]
toc: true
---

# Transaction 이슈

기존에 transaction 처리를 위해서 라이브러리 typeorm-transactional-cls-hooked를 사용하고 있었다. 하지만 typeorm의 버전이 업데이트가 되면서 이슈가 생겼다.

기존에는 custom repository 단을 만들어서 service 단에서 connection을 생성자로 주입을 해, custom repository를 불러와 사용 했었으나 문제가 생겼다.

## EntityRepository 삭제

EntityRepository가 삭제가 되면서, customRepository를 사용하는데 있어 문제가 생겼었다. 그래서 다른 방법으로 custom repository를 생성하여 사용하게 되었다.

## connection 삭제

또한 connection 클래스가 삭제가 되면서, db와 연결을 하기위해 다른 방법을 찾아봐야 했다.

# typeorm-transaction

이 라이브러리는 typeorm-transactional-cls-hooked 라이브러리를 포크하여 typeorm의 새로운 버전을 위한 라이브러리로 다시 만든 것이다. 그래서 사용 해도 되겠다는 판단이 들었다.

## DataSource

우선 AppModule에서 TypeOrmModule를 import 해올 때, 다른 방법으로 해야 했다.

typeorm라이브러리의 문서에 보면, DB와 연결을 할 때, 더이상 Connection 클래스를 사용하지 않고 DataSource 라는 클래스를 사용하게 된다.

## TypeOrmModule

AppModule에서 TypeOrmModule을 import 해올 때, DB와의 연결을 비동기적으로 연결을 한다.

```ts
@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),
    TypeOrmModule.forRootAsync({
      useClass: TypeOrmConfigService,
      dataSourceFactory: async (
        options?: DataSourceOptions
      ): Promise<DataSource> => {
        if (!options) throw new Error("options is undefined");
        return addTransactionalDataSource({
          dataSource: new DataSource(options),
        });
      },
    }),
    AuthModule,
    ForecastModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

기존에 DB를 연결하기 위해서 OrmConfig를 만들어 줬었는데, 이는 TypeOrmModuleOptions 타입으로 작성를 했었다. 하지만 forRootAsync에서는 바로 사용 할 수 없다. forRootAsync의 option으로는 이렇게 구성 되어져 있다.

```ts
export interface TypeOrmModuleAsyncOptions
  extends Pick<ModuleMetadata, "imports"> {
  name?: string;
  useExisting?: Type<TypeOrmOptionsFactory>;
  useClass?: Type<TypeOrmOptionsFactory>;
  useFactory?: (
    ...args: any[]
  ) => Promise<TypeOrmModuleOptions> | TypeOrmModuleOptions;
  dataSourceFactory?: TypeOrmDataSourceFactory;
  inject?: any[];
  extraProviders?: Provider[];
}
```

그래서 TypeOrmOptionsFactory의 인터페이스에서 createTypeOrmOption의 메소드를 이용해서 불러와야 한다.

```ts
export interface TypeOrmOptionsFactory {
  createTypeOrmOptions(
    connectionName?: string
  ): Promise<TypeOrmModuleOptions> | TypeOrmModuleOptions;
}
```

DB의 연결을 한뒤, DataSource class를 만들어 줘야 하기 때문에 dataSourceFactory 로 만들어 주면서 DB와 연결을 끝내게 된다.

## transactional 데코레이터

service단에서의 transaction을 걸어주는 방법은 기존의 방법 대로, 서비스의 메소드 마다 transactional 데코레이터를 이용해주면 된다.

```ts
@Transactional()
  async setNickName(setNickNameDto: SetNickNameDto) {}
```
