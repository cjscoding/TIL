# MongoDB

> mongoDB는 document based database이다. (json 파일과 유사한 형식이다.)

## Setup(macOS)

```bash
$ xcode-select --install
$ brew tap mongodb/brew
$ brew install mongodb-community@5.0
$ brew services start mongodb-community@5.0
```

# Mongoose

> node.js와 MongoDB 사이의 다리역할을 하는 package

## Setup

```bash
$ npm i mongoose
```

## 새로운 DB 생성하는 법

```javascript
import mongoose from "mongoose";

mongoose.connect(
  "mongodb://<terminal에 `mongo`치면 나오는 mongodb와 연결된 주소>/<내 DB 이름>"
);
```
