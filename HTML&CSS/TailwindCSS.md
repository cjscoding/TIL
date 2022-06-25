# TailwindCSS

> A utility-first CSS framework  
> 아주 많은 클래스 이름을 가지고 있는 큰 파일이라 생각하면 된다.
> 모든 클래스명을 외울 순 없으니 tailwind extension을 깔자.

## Set Up

```terminal
$ npm i -D tailwindcss postcss autoprefixer
$ npx tailwindcss init -p
```

```css
//global.css

@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
//tailwind.config.js 에서
//어느 component 혹은 page에서 tailwind를 사용할 지 설정해줘야 함

module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

## tailwind의 숫자

> 10rem == 40px

예를 들어 pt-10의 경우 padding-top을 10만큼 주라는 뜻이다.  
여기서 10은 px이 아닌 rem이다.  
브라우저에 따라 사이즈를 다르게 가질 수 있도록 rem단위를 쓰면 반응형을 디자인하기 더 좋다.

## like event delegation of js

> space-y-5

위의 클래스 명은 자식요소들에 margin-top과 margon-bottom을 5rem씩 주라는 뜻이다.  
자식 요소 각각에 margin을 설정하지 않고 마치 js의 이벤트 위임같이 부모요소에서 설정해 줄 수 있다.

## modifier

> hover:bg-teal-500

property가 true일 때의 CSS값을 설정할 수 있다.  
예를 들어, `hover:bg-teal-500`의 경우 이 자체로 하나의 클래스명이며, 마우스를 올렸을 때 백그라운드 컬러를 teal 색상으로 셋팅하겠다는 뜻이다.

> group-hover:bg-teal-500

`group`클래스명을 사용해 해당 컴포넌트 내 모든 하위 요소를 그룹화하여 스타일링을 적용할 수 있다.

> peer-invalid:text-red

input의 클래스명을 `peer`로 설정 후 연관 된 태그의 클래스명을 `peer-invalid:text-red`로 설정할 경우 input의 값이 유효하지 않을 때 해당 태그의 글씨가 red 컬러로 설정된다.

> file:hover:bg-teal-500

클래스명을 중첩으로도 사용할 수 있다.

## 반응형

> tailwind는 desktop보다 mobile 버전을 우선시한다.

> portrait

모바일의 경우 화면이 세로 방향인 경우에 맞춰 스타일링을 할 수 있다.

> landscape

모바일의 경우 화면이 가로 방향인 경우에 맞춰 스타일링을 할 수 있다.

## darkmode

> dark:hover:bg-teal-500

tailwindCSS는 브라우저의 설정값을 따라가기 때문에 예를 들어, MacOS 상 설정을 다크모드로 설정했다면 dark 클래스명이 적용된다.

```javascript
//tailwind.config.js에서

//컴퓨터의 다크모드 설정을 따를지
darkMode: "media";

//JS로 직접 토글 시킬 것인지 설정할 수 있다.
darkMode: "class";
```

다크모드를 JS로 직접 설정할 경우에는 다크모드를 적용할 컴포넌트들 중 가장 상위 컴포넌트에 `dark 클래스명을 설정해야 한다.

## Just In Time Compiler

> 코드를 실시간으로 감시하면서 필요한 클래스를 생성하는 기능

tailwindCSS 2.0까지는 웹사이트 배포를 위해 프로젝트를 빌드할 때 프로젝트에서 쓰이는 클래스명을 제외한 클래스들은 지우는 과정을 거쳤는데 tailwindCSS 3.0부터는 클래스명을 새로 생성하면 컴파일러가 해당 클래스명을 찾아 생성해주는 과정을 거친다.  
이로 인해 클래스의 중첩 사용, 특정 속성 설정, 백그라운드 이미지 설정이 가능해진 것이다.
