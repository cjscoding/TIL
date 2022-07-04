# `view "home" in views directory "/Users/cjscoding/Documents/Suetube/views"`

## pug file들을 담고있는 views 폴더를 package.json과 같은 레벨에 생성하지 않을 경우 다음과 같은 에러가 생긴다.

(위 에러는 home.pug파일을 /Suetube/src/views 폴더에 생성한 경우)

<br />

## Why?

express는 기본적으로 현재 작업 디렉토리(cwd)에서 /views 라는 디렉토리를 찾는데,
현재 작업 디렉토리를 찾는 방법은

```javascript
console.log(process.cwd());
```

를 실행시켜 보면 된다.  
`cwd: current working directory(현재 작업 디렉토리)`

실행 결과 cwd는 `package.json 파일이 존재하는 레벨의 폴더(즉, 서버를 실행시키는 파일의 위치에 따라 결정)`라는 것을 알 수 있는데, node.js가 실행되는 위치가 cwd가 되는 셈이다.

<br />

## So?

두 가지의 해결책이 있다.

### 1. /views 폴더를 서버를 실행시키는 파일(package.json)과 같은 레벨에 셋팅하기

### 2. 만약 `/src`와 같은 폴더에서 모든 코드를 관리하고 싶다면 express의 views 설정을 바꿔주면 된다.

```javascript
...

const app = express();
app.set("view engine", "pug");
app.set("views", process.cwd() + "src/views")

...
```

[참고 강의](https://nomadcoders.co/wetube/lectures/2631)
