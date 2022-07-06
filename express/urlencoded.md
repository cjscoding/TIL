## `urlencoded`

> express는 urlencoded 속성을 활용해서 form을 javascript object 형식으로 변환해준다.

```javascript
import express from "express";
const app = express();

app.use(express.urlencoded({ extended: true }));
```
