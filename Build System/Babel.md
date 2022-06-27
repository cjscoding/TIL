# Babel

> JS compiler

```
$ npm i --save-dev @babel/core
```

ES6를 지원하지 않는 브라우저를 위해 최신 JS 코드를 node가 이해할 수 있는 JS 코드로 컴파일함

## preset

> babel을 위한 거대한 plugin

```
$ npm i --save-dev @babel/core
$ npm i @babel/preset-env --save-dev
```

단순히 babel만 존재한다면 아무 동작도 실행할 수 없다. 실제 babel이 동작하는 데는 plugin의 역할이 대부분인 것이다.

새로운 ES6 기능을 ES5 문법으로 변환하기 위해서는 새 플러그인을 매번 더해야 한다.  
매번 하나하나 plugin을 추가하지 않고 bundle로 설치하기 위해 `babel preset`이라는 solution이 나왔다.

[참고 문서](https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-%EC%99%9C-babel-preset%EC%9D%B4-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80-yhk03drm7q#%EC%A7%84%EC%A7%9C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%80-babel-plugin%EC%9D%B4%EB%8B%A4)
</br>
[참고 강의](https://nomadcoders.co/wetube/lectures/2639)
