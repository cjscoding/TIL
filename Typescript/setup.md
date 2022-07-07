# Setup of TS

> 최근에는 CRA, CNA 등등 기본적으로 TS 셋팅을 함께 해주는 경우가 대부분이지만 TS의 환경을 이해하기 위해 셋업과정도 살펴보자.

1. npm, TS 설치

   ```bash
   $ npm init -y
   $ npm i -D typescript
   ```

<br />

2. ts 파일 컴파일 위한 셋팅

   ```
   // tsconfig.json

   {
       "include": ["src"],
       "compilerOptions": {
           "outDir": "build",
           "target": "ES6",
           "lib": ["ES6", "DOM"],
           "strict": true,
           "allowJs": true
       }
   }

   // package.json

   {
       ...

       "script": {
           "build": "tsc"
       }

       ...
   }

   // terminal

    $ npm run build
   ```

   `include`: 컴파일 할 ts 파일의 위치  
   `outDir`: ts파일이 컴파일된 js파일이 생성될 디렉토리  
   `target`: ts파일을 어떤 버전의 js파일로 컴파일할 지 선언  
   `lib`: ts에게 어떤 API를 사용하고 어떤 환경에서 코드를 실행할 지(런타임 환경) 선언 (타입 정의 파일을 사용하겠다고 선언)  
    ex) DOM을 선언해주지 않으면 ts파일에서 localStorage API를 사용할 때 localStorage를 이해하지 못함  
   `strict`: ts가 매우 엄격해진다. 모든 실수로부터 보호하려 에러를 띄움
   `allowJs`: ts 안에 js를 허용한다고 선언

<br />

> ts에는 `declaration files`가 존재하는데, 이 파일들에 구현되어 있는 `타입 정의는 ts가 몇몇 js 코드와 API 타입을 설명할 수 있도록 해준다`.  
> 타입 정의가 필요한 이유는 대부분의 패키지, 프레임워크, 라이브러리가 js로 구현되어 있기 때문에 ts로 이들을 사용하려 할 때 소통 오류가 생기기 때문이다.  
> 타입은 `.d.ts`이름의 파일에서 정의된다.

```typescript
// declaration file example

// declaration.d.ts
interface Config {
  url: string;
}
declare module "urlDeclaration" {
  function init(config: Config): boolean;
}

// index.ts
//urlDeclaration이 .d.ts 파일에 정의되어 있지 않으면 에러가 난다.
import { init } from "urlDeclaration";
```

<br />
만약 js 파일과 그 코드가 매우 많다면 이를 모두 ts 코드로 바꾸는 건 쉽지 않을 수 있다. 이렇게 js코드를 그대로 두고 ts의 보호(타입 체크)만 받고 싶을 때 `// @ts-check`를 사용할 수 있다.

또한 ts처럼 타입을 정의하고 싶을 때 코멘트로 이루어진 문법인 `JSDoc`을 사용할 수 있다. JSDoc은 함수 바로 위에 구현하면 된다.

```javascript
// @ts-check
/**
 * Initializes the project
 * @param {object} config
 * @param {boolean} config.debug
 * @param {string} config.url
 * @returns {boolean}
 */
export function init(config) {
  return true;
}

/**
 * Exits the program
 * @param {number} code
 * @returns {number}
 */
export function exit(code) {
  return code + 1;
}
```
