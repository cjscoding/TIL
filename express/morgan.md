# morgan

> morgan module은 middleware를 return해준다.  
> morgan은 경우에 따라 추가적인 설정(dev, combined, common, tiny)이 필요하며, 설정에 따라 request와 response에 대한 정보를 콘솔에 제공한다.

## setup

```terminal
$ npm i morgan
```

## how to use

> `morgan은 morgan("dev") 등을 호출하면 req, res, next를 포함한 함수를 return해준다.

```javascript
import express from "express";
import morgan from "morgan";

const PORT = 4000;
const app = express();
const logger = morgan("dev");
// const logger = morgan("combined");
// const logger = morgan("compose");

app.use(logger);

app.listen(PORT);
```

- dev
  - GET, path, status code 등의 정보를 가지고 있음
- combined
  - time, method, http 버전, 사용 중인 브라우저, OS 등의 정보를 가지고 있음

[참고 강의](https://nomadcoders.co/wetube/lectures/2643)
