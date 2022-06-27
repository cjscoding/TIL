# Typescript

## TS의 장점

```typescript
let fruit: string = "apple";
```

> Type의 안정성을 제공해 Bug나 Runtime error 등을 줄여 개발자 경험과 생산성을 향상시킨다.

Runtime error: 코드 실행 시 발생하는 에러

## 동작 원리

브라우저는 JS 코드만을 이해하기 때문에 compiler는 작성된 TS 코드를 JS코드 바꿔준다.

## type

TS는 항상 type을 추론하기 때문에 명시하지 않아도 되는 경우도 존재한다.

```typescript
let fruit = "apple"; // fruit의 type을 string으로 추론
```

[참고 강의](https://nomadcoders.co/typescript-for-beginners/lectures/3667)
