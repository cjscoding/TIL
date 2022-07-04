# Pug

> Pug는 템플릿 엔진으로, 템플릿을 활용해 view를 구성한다.

## Setup

```
$ npm i pug
```

## Pug & Express

pug를 `뷰 엔진으로 사용하기 위해서는 서버에게 이를 알려`주어야 하는데, express의 경우

```javascript
...

const app = express();
app.set("view engine", "pug");

...
```

라고 설정해주면 된다.

<br />

## `render`

모든 셋업이 끝나면 /views 폴더 내에 `.pug` 파일을 생성해 작성하면 된다. (파일명은 모두 소문자로 작성해야 한다.)  
/views 폴더로 생성하는 이유는 `express는 기본적으로 현재 작업 디렉토리에서 /views 라는 디렉토리를 찾기 때문`이다. 또한 /views 폴더 생성 시 package.json과 같은 레벨에서 생성하지 않으면 [디렉토리 에러](directory-error.md)가 발생한다.

pug 작성 방식은 태그를 열고 닫지 않고 들여쓰기로 부모 자식 요소를 구분하는 것이 특징이다.

```
doctype html
html(lang="ko")
    head
        title Suetube
    body
        h1 Welcome to Suetube
        footer &copy; 2022 Suetube
```

파일을 작성했으면 렌더링하기 위해서는

```javascript
const home = (req, res) => res.render("home");
```

이와 같이 `render` 속성을 사용해 렌더링 해주면 된다.
이 때 render의 인자값은 반드시 pug 파일명과 일치해야 한다.

home.pug => res.render("home");

render를 할 때 해당 페이지에 변수를 prop으로 내려줄 수도 있다.

```javascript
const home = (req, res) => res.render("home", {pageTitle: "SueTube"});
```

```
// home.pug

doctype html
html(lang="ko")
    head
        title #{pageTitle}
    body
        h1 Welcome to Suetube
        footer &copy; 2022 Suetube
```

<br />

## JS in HTML

pug는 JS를 HTML 코드 내에서 사용할 수 있다는 장점을 가지고 있다.

예를 들어 footer 코드의 년도를 JS의 getFullYear()로 구현할 수 있다.

```
footer &copy; #{new Date().getFullYear()} Suetube
```

<br />

## `include`

만약 한 프로젝트에 수십개의 페이지가 존재하고 그 페이지들에 중복되는 코드가 존재한다면 모듈화를 떠올리게 된다. pug는 이를 강점으로 가진다.

예를 들어 footer를 모듈화하는 방식을 살펴보자

```
// /src/views/partials/footer.pug

footer &copy 2022 Suetube
```

이렇게 모듈화시킨 footer 파일을

```
// /src/views/home.pug

doctype html
html(lang="ko")
    head
        title Suetube
    body
        h1 Welcome to Suetube
        include particials/footer.pug
```

이렇게 include 시켜주면 더 유지 보수에 효율적인 코드를 작성할 수 있다.

그렇다면 만약 큰 틀에서 비슷한 파일들이 많다면 어떨까?

<br />

## `extends & block`

html의 구조는 기본 틀이 거의 동일하다. 코드의 중복을 줄이기 위해 extends와 block을 사용할 수 있다.

```
// base.pug

doctype html
html(lang="ko")
    head
        block head
    body
        block content
        footer #{new Date().getFullYear()} 2022 Suetube


// home.pug에서 base.pug 파일을 상속, 확장하여 사용

extends base.pug

block head
    title Home | Suetube

block content
    h1 Home Page
```

<br />

[참고 강의](https://nomadcoders.co/wetube/lectures/2632)
