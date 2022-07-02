# Regular Expression

> 정규 표현식 (Regular Expression) 은 `일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어`이다.  
> 정규 표현식은 JS의 고유 문법이 아닌 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있는 언어이다.

## 패턴 매칭 기능

> 정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다.  
> 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

## literal

정규 표현식의 리터럴은 다음과 같다.  
_`/regexp/i`_ => `/`로 시작과 끝을 나타내며 그 사이에는 `패턴(regexp)`, 끝을 의미하는 `/` 뒤에 `플래그(i)`로 구성된다.

## method

정규 표현식을 사용하는 메서드는 다음과 같다.

- RegExp.prototype.exec
- RegExp.prototype.test
- String.prototype.match
- String.prototype.replace
- String.prototype.search
- String.prototype.split

### `RegExp.prototype.exec`

> exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색해 `매칭 결과를 배열로 반환`한다. `매칭 결과가 없는 경우에는 null을 반환`한다.

```javascript
const str = "Is this all there is?";
const regExp = /is/;

regExp.exec(str); // ["is", index: 5, input: "Is this all there is", groups: undefined]
```

### `RegExp.prototype.test`

> test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 `매칭 결과를 Boolean값으로 반환`한다.

```javascript
const str = "Is this all there is?";
const regExp = /is/;

regExp.test(str); // true
```

### `String.prototype.match`

> match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```javascript
const str = "Is this all there is?";
const regExp = /is/;

regExp.match(str); // ["is", index: 5, input: "Is this all there is", groups: undefined]
```

exec 메서드와 match 메서드가 같아 보일 수 있지만 차이는 g 플래그를 쓸 때 나타난다.  
먼저 플래그를 살펴보자.

## flag

> 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

| 플래그 | 의미        | 설명                                                              |
| ------ | ----------- | ----------------------------------------------------------------- |
| i      | Ignore case | `대소문자를 구별하지 않고` 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 `모든 문자열을 전역 검색`한다. |
| m      | Multi line  | `문자열의 행이 바뀌더라도` 패턴 검색을 계속한다.                  |

플래그를 사용하지 않을 경우(default)에는 대소문자를 구별해서 패턴을 검색한다.

### 그렇다면 다시 `exec 메서드와 match 메서드의 차이`를 살펴보자.

```javascript
const str = "Is this all there is?";
const regExp = /is/g;

regExp.exec(str); // ["is", index: 5, input: "Is this all there is", groups: undefined]
regExp.match(str); // ["is", "is"]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.  
하지만 match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

## pattern

> 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.  
> 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 경우 이를 `정규 표현식과 매치(match)한다.`고 한다.

### 임의의 문자열 검색

> `.`은 임의의 문자 한 개를 의미한다.

```javascript
const str = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색
const regExp = /.../g;

regExp.match(str); // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 반복 검색

> `{m, n}`은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.

> `{n}` 은 앞선 패턴이 n번 반복되는 문자열을 의미한다. `== {n, n}`

> `{n,}` 은 앞선 패턴이 최소 n번 반복되는 문자열을 의미한다.

> `+` 은 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. `== {1,}`

> `?` 은 앞선 패턴이 최대 한 번 반복되는 문자열을 의미한다. `== {0, 1}`

### OR 검색

> `|` 은 or의 의미로, 예를 들어 `/A|B|g` 는 'A' 또는 'B'를 전역 검색함을 의미한다.

> `[]` 내의 문자는 or로 동작한다. 또한 범위는 `-` 를 사용해 지정할 수 있다.  
> 예를 들어, `/[0-9,]+/g` 는 '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다는 뜻이다.

> `^` 은 not의 의미를 갖는다.

> `[\d]` 는 숫자를 의미한다. `== [0-9]`  
> `[\D] == [^\d] == [^0-9]`

> `[\w]` 는 알파벳, 숫자, 언더스코어를 의미한다. `== [A-Za-z0-9_]`  
> `[\W] == [^\w] == [^A-Za-z0-9_]`

> `\s` 는 스페이스, 탭 등 여러 가지 공백 문자를 의미한다.

> `*` 는 all을 의미한다. 어떤 문자열이든, 어떤 길이로든 올 수 있다.
> 단, `*` 앞에 `()`로 조건이 있다면 그 조건에 해당하는 뭐든 올 수 있다는 뜻

### 시작, 끝 위치 검색

> [] 내의 ^은 not의 의미를 가지지만, `[] 밖에서의 ^는 문자열의 시작을 의미`한다.

> `^` 은 시작, `$` 은 끝을 의미한다.

## example

### 특정 단어로 시작하는지 검사

```javascript
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사
const regExp = /^https?:\/\//;

regExp.test(url); // true
```

### 특정 단어로 끝나는지 검사

```javascript
const fileName = "index.html";

// 'html'로 끝나는지 검사
const regExp = /html$/;

regExp.test(fileName); // true
```

### 숫자로만 이루어진 문자열인지 검사

```javascript
const target = "12345";

const regExp = /^\d+$/;

regExp.test(target); // true
```

### 하나 이상의 공백으로 시작하는지 검사

```javascript
const target = " Hi";

const regExp = /^[\s]+/;

regExp.test(target); // true
```

### 아이디로 사용 가능한 지 검사

```javascript
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
const regExp = /^[A-Za-z0-9]{4, 10}$/;

regExp.test(id); // true
```

### 메일 주소 형식에 맞는지 검사

```javascript
const email = "devSue22@gmail.com";

// 알파벳 대소문자 또는 숫자로 시작하고, 알파벳 대소문자 또는 숫자에 -, _, .를 포함하여 모든 올 수 있고, @가 오고, 알파벳 대소문자 또는 숫자로 시작하고, 알파벳 대소문자 또는 숫자에 -, _, .를 포함하여 모든 올 수 있고, .이 오고, 알파벳 대소문자 2~3자리에 해당하는 문자열로 끝나야 한다.
const regExp =
  /^[A-Za-z0-9]([-_\.]?[A-Za-z0-9])*@[A-Za-z0-9])[-_\.]?[A-Za-z0-9]*\.[A-Za-z]{2, 3}$/;

regExp.test(email); // true
```

### 핸드폰 번호 형식에 맞는지 검사

```javascript
const phoneNumber = "010-1234-5678";

const regExp = /^\d{3}-\d{4}-\d{4}$/;

regExp.test(phoneNumber); // true
```

### 특수 문자 제거하기

```javascript
const target = "abc#123";

const regExp = /^[A-Za-z0-9]/gi;

target.replace(regExp, ""); // abc123
```

참고 도서 - 모던 자바스크립트 Deep Dive 31장. RegExp  
[참고 강의](https://nomadcoders.co/wetube/lectures/2658)
