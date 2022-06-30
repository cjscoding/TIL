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

## Polymorphism & generic

> generic은 type의 placeholder라고 볼 수 있다.  
> generic을 활용해 다형성 함수를 구현할 수 있다.

### case1

```typescript
type FindFirstElement = {
  // bad way
  //   (arr: string[]): string
  //   (arr: number[]): number
  //   (arr: (string | boolean | number)[]): string | boolean | number

  // good way
  <T>(arr: T[]): T;
};

const findFirstElement: FindFirstElement = (arr) => arr[0];

const num = findFirstElement([1, 2, 3]);
const str = findFirstElement(["a", "b", "c"]);
const poly = findFirstElement(["a", 1, true]);
```

=> 모든 타입의 조합을 적어줄 필요없이 ts는 type을 추론해서 추론한 type을 call signature로 삼는다.

### case2

> 실제로 많이 사용하는 스타일

```typescript
type Person<E> = {
  name: string;
  extraInfo: E;
};

const sue: Person<{}> = {
  name: "Sue",
  extraInfo: {
    favColor: "yellow",
  },
};

const jisu: Person<null> = {
  name: "Jisu",
  extraInfo: null,
};
```

=> type은 재사용이 가능하다.

```typescript
function findFirstElement(arr: Array<number>) {
  console.log(arr[0]);
}
```

=> 제네릭으로 인자의 타입을 정할 수 있다.

## example code

### 배열의 마지막 요소를 리턴하기

```typescript
type LastElement = {
  <T>(arr: T[]): T;
};

const last: LastElement = (arr) => {
  return arr[arr.length - 1];
};

let a = last([1, "a", true]);
console.log(a); // true
```

### 배열의 시작에 새로운 요소를 넣어 리턴하기

```typescript
type MakeNewArray = {
  <T, M>(arr: T[], item: M): (T | M)[];
};

const prepend: MakeNewArray = (arr, item) => {
  return [item, ...arr];
};

let b = prepend([1, "a", 3], true);
console.log(b); // [true, 1, "a", 3]
```

[참고 강의](https://nomadcoders.co/typescript-for-beginners/lectures/3675)
