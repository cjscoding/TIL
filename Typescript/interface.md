# Interface of TS

## interface의 역할

> interface의 유일한 역할은 object의 shape을 특정하는 것이다.  
> 즉, `TS에게 object의 shape을 설명`해준다. 같은 맥락으로 클래스가 지정한 메서드와 프로퍼티를 갖도록 강제할 수 있다.

```typescript
interface User {
  name: string;
}

interface Player extends User {
  role: string;
}

const sue: Player = {
  name: "sue",
  role: "FE",
};
```

> type을 사용해 위 코드를 다시 작성해보면 다음과 같다.

```typescript
type User = {
  name: string;
};
type Player = User & {
  role: string;
};
const sue: Player = {
  name: "sue",
  role: "FE",
};
```

=> TS에게 object의 shape을 설명할 때 interface, type 모두 사용 가능하다.

## 같은 이름의 interfaces

```typescript
interface User {
  name: string;
}
interface User {
  lastName: string;
}
interface User {
  age: number;
}

const sue: User = {
  name: "sue",
  lastName: "choi",
  age: 26,
};
```

이와 같이 같은 이름의 interface가 여러 개인 경우 TS는 이를 하나로 합쳐준다.
(Type의 경우에는 에러가 난다.)

## interface의 장점

추상 클래스로 구현된 코드를 JS로 컴파일하면 어떻게 될까?

```typescript
abstract class User {
  constructor(protected firstName: string, protected lastName: string) {}
  abstract fullName(): string;
  abstract sayHi(name: string): string;
}
class Player extends User {
  fullName(name: string) {
    return `${this.firstName} ${this.lastName}`;
  }
  sayHi() {
    return `Hello ${name}. My name is ${this.fullName()}`;
  }
}
```

=> compiled

```javascript
"use strict";
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
class Player extends User {
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
  sayHi(name) {
    return `Hello ${this.name}. My name is ${this.fullName()}`;
  }
}
```

추상클래스를 사용하는 이유는 표준화된 프로퍼티와 메서드를 갖도록 하는 청사진을 만들기 위해서이다. 만약 추상 클래스를 다른 클래스들이 특정 모양을 따르도록 하기 위한 용도로 쓴다면 javascript 코드에서의 `class User`는 사용되지 않는 코드이고 결과적으로 JS 파일을 불필요하게 무겁게 만든다.

_`interface는 이를 해결할 수 있다`_

인터페이스는 컴파일 후 JS 코드로 변환되지 않고 사라진다.

```typescript
interface User {
  fistName: string;
  latName: string;
  fullName(): string;
  sayHi(name: string): string;
}
interface Human {
  health: number;
}
class Player implements User, Human {
  constructor(
    public firstName: string,
    public lastName: string,
    public health: number
  ) {}
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
  sayHi(name: string) {
    return `Hello ${name}. My name is ${this.fullName()}`;
  }
}
```

=> compiled

```javascript
"use strict";
class Player {
  contructor(firstName, lastName, health) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.health = health;
  }
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
  sayHi(name) {
    return `Hello ${name}. My name is ${this.fullName()}`;
  }
}
```

이와 같이 `interface를 사용`하여 컴파일된 JS 코드를 보면 `추상 클래스를 추가로 사용하고 있지 않다`는 것을 확인할 수 있다. 또 JS 코드로 컴파일되지 않았기에 JS 파일이 가벼워진다.

`implements`  
interface 또는 type을 상속하기 위한 키워드 (type도 상속이 가능하다.)

## interface의 단점

앞서 다룬 내용처럼 인터페이스는 가볍고 클래스의 생김새를 정의해주며 다중으로 상속이 가능하다는 장점이 있지만 단점 또한 존재한다.

1. 인터페이스를 상속하면 private 프로퍼티를 사용할 수 없다.
2. 추상 클래스에는 constructor가 존재하지만 인터페이스에는 존재하지 않기 때문에 인터페이스를 상속받는 클래스에서 contructor를 구현해야 한다.

[참고 강의](https://nomadcoders.co/typescript-for-beginners/lectures/3680)
