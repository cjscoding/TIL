# Prisma

## What is Prisma

Node.js와 Typescript `ORM(Object Relational Mapping)`으로 JS 또는 TS 코드와 DB 사이에 다리를 놓아주는 역할(like translator)을 한다.

SQL문과 같은 DB 언어를 사용하지 않고 미리 오류를 발견해주는 TS로만 코드를 짜는 것이 더 편하기 때문에 사용한다.
즉, `Prisma를 사용해 더 편리하게 DB를 사용`할 수 있다.

Prisma를 사용하기 위해서는 먼저 `schema.prisma`파일에 DB가 어떻게 생겼는지 명시해주어야 한다.

## Prisma의 장점

Prisma는 DB와 상호작용을 도울 뿐만아니라 client를 생성해 `자동완성` 기능 또한 제공한다.
Visual Database Browser인 `Prisma Studio`를 제공한다. DB를 위한 관리자 패널이라고 볼 수 있다.

## Setup

```terminal
// 설치 시
$ npm i prisma -D

// prisma 최초 사용 시
$ npx prisma init

// prisma 사용 시
$ npx prisma
```

### `npx prisma init` 실행 시 콘솔에 찍인 가이드를 따라 초기 셋팅을 해야 한다.

1. `.env` 파일에 있는 `DATABASE_URL`을 설정해야 한다.

   ```
   // .env 파일
   DATABASE_URL="mysql://<URL>/<DB-name>"
   ```

   - prisma는 ts와 DB의 번역기 역할이기 때문에 따로 DB가 필요하다.
   - URL과 DB-name를 설정하는 방식은 이어지는 `PlanetScale Setup` 파트에서 확인할 수 있다.

2. `schema.prisma` 파일에서 `datasource의 provider`를 설정해야 한다.

   ```prisma
   // schema.prisma 파일에서 DB를 MySQL로 설정
   datasource db {
     provider = "mysql"
     url      = env("DATABASE_URL")
   }
   ```

   - provider는 사용할 DB라고 보면 된다. Prisma는 provider로 postgresql, MySQL, SQLite, SQLServer, mongoDB를 지원한다.

### 자동정렬 위한 setting

vscode settings.json 파일에 아래 설정값 추가

```
"[prisma]": {
"editor.defaultFormatter": "Prisma.prisma"
}

```

## What is PlanetScale

PlanetScale는 MySQL과 호환되는 serverless DB 플랫폼이다.

새 model을 추가하려 할 때 마치 `git`을 사용하는 것처럼 branch를 내서 구현하는 식으로 DB를 다룰 수 있다는 장점이 있다.

### PlanetScale Setup

CLI Setup: [OS에 따른 CLI 설치 명령어](https://github.com/planetscale/cli)

```
// region list
$ pscale region list

// create DB
$ pscale database create <database-name> --region <one of the region list>

//
```

DB 플랫폼(ex. AWS, Heroku)에서는 DB를 하나 만들 떄마다 암호를 생성한다. 그리고 이 암호를 .env 등의 환경파일에 저장한다. 하지만 만약 누군가 컴퓨터를 열어 이를 확인하거나 실수로 git에 환경파일을 올리게 될 경우 암호가 유출될 수 있다는 위험이 존재한다.

따라서 `일반적으로 쓰이는 방식은 두 개의 DB를 사용하는 것`이다. 컴퓨터에서 server를 작동할 수 있도록 가짜 DB를 사용하고, 실제 배포할 때는 AWS나 Heroku 등을 사용한다.

위 방식을 대신해 PlanetScale에서는 컴퓨터와 PlanetScale 사이에 `일종의 보안 tunnel`을 이용할 수 있도록 한다. 이로 인해 `MySQL을 설치 및 실행할 필요도, .env 파일에 암호를 저장할 필요도 없어`진다.

그렇다면 PlanetScale에서는 어떻게 암호 없이 컴퓨터와 PlanetScale 사이에 보안 연결을 만들 수 있을까?

답은 CLI에 있다.

```
$ pscale connect <database-name>
```

위 명령어를 실행하면 해당 database와 연결되며 `PlanetScale server와 연결된 URL`이 콘솔창에 출력된다.

### Vitess

PlanetScale은 스스로를 `MySQL-compatible serverless database platform`으로 소개하는데, compatible(호환되는)이라 지칭하는 이유는 `PlanetScale은 오픈소스인 Vitess를 기반`으로 하고 있기 때문이다.

Vitess를 기반으로 하기 떄문에 발생하는 문제가 있는데, 아래 코드를 살펴보자.

```prisma
model User {
    id        Int      @id @default(autoincrement())
    phone     Int?     @unique
    email     String?  @unique
    name      String
    avatar    String?
    createdAt DateTime @default(now())
    updatedAt DateTime @updatedAt
}

model Comment {
    id      Int    @id @default(autoincrement())
    content String
    userId  Int
}
```

사용자가 댓글을 달게 되면 Comment DB의 userId값이 User DB에 존재하는 id값인지 확인한 후 로직을 승인하는 것이 일반적이다.

하지만, PlanetScale이 DB로 사용하고 있는 `Vitess는 DB를 잘게 쪼개서 여러 server에 분산시키는 데 특화`되어 있는데, 이 때문에 기존 MySQL 방식과 달리 `foreign key constraint(외래키 제약)를 지원하지 않는다`.

따라서 User DB 상의 존재 여부를 확인하지 않고 댓글을 다는 로직을 승인한다. 이와 같은 로직들이 아무런 문제없이 작동하는 것은 회원 관리 등의 차원에서 좋지 않다.

Prisma는 이 문제를 해결하는 데 도움을 줄 수 있다.

즉, `한 객체가 다른 객체에 연결되어 있는 상태라는 것을 보장`받는 데 prisma의 도움을 받을 수 있다. 방식은 다음과 같다.

```prisma
// schema.prisma
//1
generator client {
    provider        = "prisma-client-js"
    previewFeatures = ["referentialIntegrity"]
}
// 2
datasource db {
    provider             = "mysql"
    url                  = env("DATABASE_URL")
    referentialIntegrity = "prisma"
}
```

1. `previewFeatures` 옵션을 설정해준다. 다른 객체에 연결될 때 그 객체가 존재하는지 확인할 수 있다.
2. `referentialIntegrity` 작업을 prisma가 할거라 선언해준다.

## Push Schema

### 지금까지의 Setup을 잘 마쳤다면 생성한 Schema들을 PlanetScale에 Push해보자.

```terminal
$ npx prisma db push
```

> `(주의사항)` 이 때 PlanetScale은 계속 실행된 상태여야 한다. 따라서 새로운 터미널을 열어 실행해야한다.

database가 prisma schema에 sync되었다는 메시지와 prisma client가 생성되었다는 메시지가 뜨면 성공이다. (schema는 [planetscale](https://app.planetscale.com/)에서 확인할 수 있다.)

## Prisma Client

```terminal
$ npm i @prisma/client
```

Prisma Client는 schema에서 작성한 data를 기반으로 한 자동완성 기능을 제공한다.

예를 들어

```
// schema.prisma
model User {
    id        Int      @id @default(autoincrement())
    phone     Int?     @unique
    email     String?  @unique
    name      String
    avatar    String?
    createdAt DateTime @default(now())
    updatedAt DateTime @updatedAt
}
```

prisma client가 생성되면 위 schema를 `node_modules/.prisma/client/index.d.ts` 내에 TS로 schema.prisma를 기반으로 한 type이 생성된다.

```typescript
export type User = {
  id: number;
  phone: number | null;
  email: string | null;
  name: string;
  avatar: string | null;
  createdAt: Date;
  updatedAt: Date;
};
```

> DB Admin Panel 명령어

```terminal
$ npx prisma studio
```

```typescript
// libs/client.ts
import { PrismaClient } from "@prisma/client";

const client = new PrismaClient();

// 생성하는 과정에서 각각의 data의 오류를 찾아내 알려준다.
client.user.create({
  data: {
    name: "sue",
    email: "devSue22@gmail.com",
  },
});
```

만약 위 client를 pages/index.tsx 등의 프론트엔드단 파일에서 import하게 되면 error가 난다. 즉, `브라우저에서는 실행할 수 없는 파일`이라는 뜻이다. 이로 인해 client단에서 쉽게 DB에 접근하는 것을 막아 보안이 된다.

그렇다면 DB를 활용하기 위해 따로 서버를 구축해야 할까?

Next.js를 기반으로 한다면 대답은 `NO`이다.  
Next.js에서는 API를 만들기 위해 다른 서버를 구축할 필요가 없다.

## API for Prisma

> Next.js에서는 pages/api/ 폴더에서 API를 만들 수 있다. (api route에서 client에 data를 보내주는 것)  
> 그래서 Next.js를 풀스택 프레임워크라 한다.

```typescript
// pages/api/client-test.tsx
import type { NextApiRequest, NextApiResponse } from "next";
import client from "../../libs/client";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  await client.user.create({
    data: {
      name: "sue",
      email: "devSue22@gmail.com",
    },
  });
  res.json({
    ok: true,
  });
}
```

/api/client-test url에 접속 후 planetscale 관리자 패널에서 새로 생긴 user data를 확인할 수 있다.

[참고 강의](https://nomadcoders.co/carrot-market/lectures/3503)
