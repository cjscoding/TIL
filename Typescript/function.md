# Function of TS

## call signiture

> TS에게 이 함수가 어떻게 호출될지 설명해주는 부분

```typescript
type Add = (a: number, b: number) => number; // call signiture

const addFunc: Add = (a, b) => a + b;
```

## overloading

> 함수가 여러개의 call signiture을 가질 때 발생한다.
> 여러개의 call signiture는 다른 타입 또는 파라미터 개수가 다른 경우들이 있다.

```typescript
type Add = {
  (a: number, b: number): number;
  (a: number, b: number, c: number): number;
};

const add: Add = (a, b, c?: number) => {
  if (c) return a + b + c;
  return a + b;
};
```
