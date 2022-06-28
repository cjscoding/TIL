# Type of Typescript

！ 변수에 값을 할당해주는 경우에는 Typescript가 추론하도록 Type을 지정해주지 않아도 된다.

## property

### readonly

> 오로지 읽을 수만 있는 변수로 지정한다. 수정하려 할 시 Typescript가 동작을 멈춘다.

```typescript
type Name = string;
type Age = number;
type Person = {
  readonly name: Name;
  age?: Age;
};

const targetPerson = (name: string): Person => ({ name });
const sue = targetPerson("sue");

sue.age = 26; // works
sue.name = "jisu"; // doesn't work
```

### Tuple

> 여러 타입의 변수를 모아 배열을 만들 수 있다. 각 배열 내 요소들은 반드시 지정된 타입과 순서를 맞춰야한다.

```typescript
const Person: [string, number, boolean] = ["sue", 1, true];
```

### any

> typescript의 감시에서 벗어나도록 해준다.  
> 불가피한 상황이 아니라면 되도록 사용하지 않는 것이 좋다.

```typescript
const num_array: any[] = [1, 2, 3];
const bool_value: any = true;
num_array + bool_value; // works
```

### unknown

> 예를 들어 API 요청으로 받을 data의 type을 모를 경우와 같이 typescript에게 type을 알려줄 수 없을 때 사용한다.

```typescript
let a: unknown;

if (typeof a === "number") let b = a + 1;
if (typeof a === "string") let b = a.toUpperCase();
```

### void

> 아무것도 return하지 않는 함수를 대상으로 쓰인다.  
> void라고 지정하지 않아도 함수의 return값이 없으면 typescript는 void type으로 인식한다.

```typescript
function sayHello() {
  console.log("Hello");
}
```

### never

> 예를 들어 예외가 발생한 상황일 경우 등에 함수가 절대 return을 하지 않을 것을 명시해주기 위해 사용한다.

```typescript
function sayHello(name: string | number): never {
  if (typeof name === "string") {
    console.log(`Hello ${name}`);
  } else if (typeof name === "number") {
    console.log(`${name} Hello`);
  } else {
    throw new Error("wrong type");
  }
}
```

[참고 강의](https://nomadcoders.co/typescript-for-beginners/lectures/3670)
