# Classes of TS

## Difference with JS

```javascript
class User {
    constructor(firstName, lastName, nickName){
        this.firstName = firstName,
        this.lastName = lastName,
        this.nickName = nickName,
    }
}
```

```typescript
class User {
  constructor(
    private firstName: string,
    protected lastName: string,
    public nickName: string
  ) {}
}
```

생성자를 만들 때 TS에서는 JS처럼 `this.sth = sth` 형식으로 명시하지 않고 `접근 제어자(public, protected, private..), 이름, 타입을 명시`하면 된다.

## Access Modifier(접근 제어자)

`public`  
property를 public으로 설정한 경우 외부 어느 곳에서든 접근 가능하다.

`private`  
property를 private으로 설정한 경우 외부는 물론 해당 클래스를 상속한 클래스에서도 접근할 수 없다.

`protected`  
property를 protected로 설정한 경우 외부에서는 접근할 수 없지만 해당 클래스를 상속한 클래스 내에서는 접근 가능하다.

## abstract class

> 추상클래스는 다른 클래스가 extends 키워드를 사용해 상속받을 수 있는 클래스이다.  
> 하지만 직접 인스턴스를 생성할 수는 없다.

## abstract method

> 추상메서드는 추상클래스 안에 존재하는 함수로, 추상클래스를 상속받는 모든 클래스에서는 추상메서드를 구현해야한다.  
> 추상메서드는 call signiture로 표현해야한다.

```typescript
abstract class User {
  constructor(
    protected firstName: string,
    protected lastName: string,
    protected nickName: string
  ) {}
  abstract getNickName(): void;

  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

class Player extends User {
  getNickName() {
    console.log(this.nickName);
  }
}

const sue = new Player("jisu", "choi", "sue");

sue.getFullName();
```

## class는 type처럼 사용할 수 있다.

```typescript
type Words = {
  [key: string]: string;
};

class Dict {
  private words: Words;
  constructor() {
    this.words = {}; // 직접 초기화
  }
  // 클래스를 타입으로 사용
  add(word: Word) {
    if (this.words[word.term] === undefined) {
      this.words[word.term] = word.def;
    }
  }
  def(term: string) {
    return this.words[term];
  }
}

class Word {
  constructor(public term: string, public def: string) {}
}

const apple = new Word("apple", "fruit");

const dist = new Dist();

dict.add(apple);
dict.def("apple");
```

[참고 강의](https://nomadcoders.co/typescript-for-beginners/lectures/3678)
