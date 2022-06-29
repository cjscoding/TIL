# Middleware

> request와 response 사이의 software라고 할 수 있다.  
> 모든 controller는 middleware가 될 수 있다.

```javascript
import express from "express";
const app = express();

//sayHello is middleware in this code

const sayHello = (req, res, next) => {
  console.log("Hello");
  next();
};

const sayBye = (req, res) => {
  return res.send("<h1>Bye</h1>");
};

app.get("/", sayHello, sayBye);
```

- middleware는 response를 하지 않는다. request를 지속시켜준다.
- middleware next 함수를 호출하기도, res.send를 사용하기도 한다.

## global middleware

> app.use를 활용해 전역으로 사용 가능한 middleware를 만들 수 있다.

```javascript
import express from "express";
const app = express();

const sayHello = (req, res, next) => {
  console.log("Hello");
  next();
};

const sayWelcome = (req, res, next) => {
  console.log("Welcome");
  next();
};

const sayBye = (req, res) => {
  return res.send("<h1>Bye</h1>");
};

app.use(sayHello); // 1
app.use(sayWelcome); // 2
app.get("/", sayBye); // 3
app.get("/login", sayBye); // 3("/" get 요청)이 아니라면 4
```

- middleware는 정의된 순서에 따라 실행된다. 즉 middleware를 global하게 쓰고 싶다면 가장 위에 정의해야한다.
- 위 코드상 sayBye 함수와 같이 마지막 controller에는 next함수를 잘 쓰지 않는다.

## private middleware

> 조건에 따라 send나 next함수를 호출한다.

```javascript
const privateMiddleware = (req, res, next) => {
  const url = req.url;
  if (url === "/protected") {
    return res.send("<h1>Is protected</h1>");
  }
  next();
};
```
