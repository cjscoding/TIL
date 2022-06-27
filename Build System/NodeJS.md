# ☊ NodeJS

> Browser 밖에서 동작하는 JS

# ☋ npm

> JS를 위한 Package manager

npm은 nodeJS과의 상호작용을 도와준다. (그래서 nodeJS 설치하면 npm이 함께 설치된다.)

```
$ npm init
```

- package.json을 만들어주는 command

# Nodemon

> 파일이 수정될 때마다 재시작을 해줌 (수정 때마다 npm run dev를 할 필요가 없음)

```json
//package.json

"scripts": {
    "dev": "nodemon --exec babel-node index.js"
  },
```

[참고 강의](https://nomadcoders.co/wetube/lectures/2667)
