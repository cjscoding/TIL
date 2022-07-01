# express routing

> router: url이 어떻게 시작하느냐에 따라 구분하는 방법

example code

```javascript
// src/routers/globalRouter.js

import express from "express";

const app = express();
const globalRouter = express.Router();
const userRouter = express.Router();

app.use("/", globalRouter);
app.use("/user", userRouter);

const handleHome = (req, res) => res.send("Home");
const handleJoin = (req, res) => res.send("Join");

globalRouter.get("/", handleHome);
userRouter.get("/join", handleJoin); // url: /user/join
```

[참고 강의](https://nomadcoders.co/wetube/lectures/2657)
