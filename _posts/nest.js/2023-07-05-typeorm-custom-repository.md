---
layout: single
title: "[TypeOrm] Custom Repository"
categories: NestJs
tag: [Nest.js, typeorm, Side-Project]
toc: true
---

# TypeOrm 0.3

사이드 프로젝트를 진행 하면서 TypeOrm의 버전이 0.3 버전으로 변경이 되면서, 생긴 일이다.

나는 개인적으로 코드의 보수 및 가독성을 위해서 service단과 repository 단을 나누어 관리를 하는 것을 선호한다.

그래서 custom repository를 만들어 DB에 접근하는 쿼리문을 관리를 하려고 만들려고 했었다.

하지만 EntityRepository 데코레이터가 사라져 있다는 것을 발견했다. 그래서 다른 방법으로 만들어야 했다.

# Custom Repository

먼저 Custom Repository란 하나의 Entity를 특정하여 class를 만드는 것이다. 그래서 EntityRepository라는 데코레이터를 사용하고, 특정한 Entity를 Repository라는 클래스에 제너릭으로 받아 상속을 해주면 된다.

하지만 EntityRepository라는 데코레이터가 없으니 따로 만들어 줘야 한다.

여러가지의 방법을 찾아 봤는데 하나는 이 데코레이터를 만들어 주는 방법이었다. 하지만 너무나 복잡해 보이고, 시간이 많은 편이 아니라서 이 방법은 채택하지 않았다.

대신 다른 간단한 방법을 찾아내어 사용하였다.

## Repository

```ts
@Injectable()
export class AuthRepository extends Repository<User> {
  constructor(private dataSource: DataSource) {
    super(User, dataSource.createEntityManager());
  }

```

기존에 custom repository를 만드는 것과 매우 유사하다.
클래스를 만들고 repository를 상속 받으면 된다. 하지만 여기서 중요한 것은 생성자 부분이다.

Repository class의 생성자는 target과 manager를 필수로 전달을 받아야 한다.

```ts
    constructor(target: EntityTarget<Entity>, manager: EntityManager, queryRunner?: QueryRunner);
```

그렇기 때문에 target에 해당하는 entity를 전달 해주고, manager로 적절하게 받아서 사용하면 된다.

이처럼 custom repository를 만들었다면, provider로 등록하여 사용하면 된다.
