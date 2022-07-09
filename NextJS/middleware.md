# Middleware of NextJS

> NextJS는 Middleware를 지원한다.

`_middleware.ts` (혹은 js) 으로 파일명을 생성하여 미들웨어 코드를 구현하면 된다.

/pages 폴더 내 가장 상위 레벨에 존재한다면 global로 그 하위 폴더에 존재한다면 각 폴더의 controller가 동작하기 전 미들웨어로 작동한다.

미들웨어에는 두 가지 argument가 존재한다. 하나는 `request`(요청), 하나는 `event`(이벤트)이다.

ts를 사용한다면 이 각각은 `NextRequest`(=>requst), `NextFetchEvent`(=>event) 데이터 타입을 불러온다.

```typescript
//_middleware.ts
import type { NextRequest, NextFetchEvent } from "next/server";

export function middleware(req: NextRequest, ev: NextFetchEvent) {
    console.log("I'm Middleware!!);
}
```
