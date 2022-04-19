# NextJS 시작하기

[니꼬쌤과 NextJS 시작하기](https://nomadcoders.co/nextjs-fundamentals/lectures/3437)

​          

#### **목차**

:one: **FRAMEWORK OVERVIEW**

1. [library와 framework의 차이점](#library와-framework의-차이점)

2. [Pages](#pages)

3. [Static Pre Rendering (사전 렌더링)](#static-pre-rendering-(사전-렌더링))

4. [Routing](#routing)

5. [CSS Modules](#css-modules)

6. [Styled JSX](#styled-jsx)

7. [Custom App](#custom-app)

​                      

:two: **PRACTICE PROJECT**

​	1. [Patterns](#patterns)

​              

-----

​           

# #1 FRAMEWORK OVERVIEW

​              

## library와 framework의 차이점

#### library

- 우리가 가져다가 쓰는 것
- 사용자가 파일 이름이나 구조 등을 정하고, 모든 결정을 내림

#### framework

- 정해진 틀 안에서 커스터마이징
- 파일 이름이나 구조 등을 정해진 규칙에 따라 만들고 따름
- 코드를 올바른 위치에 둔다면 framework가 코드 불러서 사용 (ReactDOM.render 노필요 !)

​         

#### 차이점

Inversion of Control (통제의 역전)

library에서 메서드를 호출하면 사용자가 제어할 수 있지만, framework에서는 제어가 역전되어 framework에서 사용자를 호출함

​                

-----

​              

## Pages

`create react-app`으로 프로젝트 생성하면 `react-router-dom` 설치 -> router 생성 -> component import -> router render 등 할게 많지만 

NextJS를 쓰면 필요없이 시간 절약 가능 ! 

또한 404 error page 따로 생성할 필요 없음

​             

### Pages 파일명 규칙

pages 폴더 안의 **파일명에 따라 route가 결정됨** 

즉, 파일명이 URL이 되는 것

component의 이름은 중요하지 않지만,  `export default function`은 반드시 해줘야함 

​         

### 예외사항

`index.js` 는 App의 시작하는 파일로 `localhost:3000 ` 로 나타남

`localhost:3000/index `의 페이지는 존재하지 않음

### +

jsx를 쓰고있다면 React.js를 import할 필요 없음 

하지만 `useState` , `uesEffect`, `lifecycle method` 등을 사용해야 할 때는 import 필수

​                

-----

​              

## Static Pre Rendering (사전 렌더링)

next.js의 가장 좋은 기능 중 하나는 App에 있는 페이지들이 **정적으로 미리 렌더링되는 것**

-> 미리 생성한 HTML 파일을 초기 로드 시 표시함

기존의 퓨어한 React.js는 요청을 받으면 렌더링 되는 것

-> 초기 로드시 빈 화면이 나오고, 페이지가 모두 로드되는 속도가 느림

​              

#### hydration

react에서 server side rendering 혹은 SSG(static site generation)을 실행한 HTML 결과물을 받아온 뒤, 브라우저에서 이것을 다시 react tree에 맞게 파싱하는 것

​              

---------

​              

## Routing

NextJS App에서 anchor tag(`<a href=""></a>`)를 navigating 하는 데에 사용하면 안되는 이유는 App 내에서 navigate할 때 사용해야만 하는 특정 component가 존재하기 때문

또한 브라우저가 다른 페이지로 이동하기 위해 전체 페이지가 새로고침 되어 느려지는 것을 볼 수 있음

​              

`import Link from "next/Link";` 를 사용해 NextJS에서 제공하는 client side navigation 사용

```javascript
<nav>
	<Link href="/">
		<a>Home</a>
	</Link>
	<Link href="/about">
		<a>About</a>
	</Link>
</nav>
```

-> 새로고침되지 않고 페이지를 불러옴 !

​           

`Link`에 className, style를 직접 지정해줘도 적용되지 않음 

anchor tag에 모두 작성해주면 됨 

​           

### useRouter()

App의 함수 component에서 router 객체 내부에 접근하려면 useRouter() hook 사용할 수 있음

```javascript
const router = useRouter();

  return (
    <nav>
      <Link href="/">
        <a style={{ color: router.pathname === "/" ? "red" : "blue" }}>Home</a>
      </Link>
      <Link href="/about">
        <a style={{ color: router.pathname === "/about" ? "red" : "blue" }}>
          About
        </a>
      </Link>
    </nav>
  );
```

​           

------

​            

## CSS Modules

### 사용법

`NavBar.module.css` 형태로 생성 (파일명 상관 없음 .module.css 형태만 지켜주면 됨)

css 적용할 파일에서 `import styles from "./NavBar.module.css"` 

`<nav className={styles.nav}></nav>`로 사용할 수 있음

`<a className={router.pathname === "/" ? styles.active : ""}>Home</a>`

​           

- 두 개 이상 class style 지정할 때 방법

```js
return (
    <nav>
      <Link href="/">
        <a
          className={`${styles.link} ${
            router.pathname === "/" ? styles.active : ""
          }`}
        >
          Home
        </a>
      </Link>
      <Link href="/about">
        <a
          className={[
            styles.link,
            router.pathname === "/about" ? styles.active : "",
          ].join(" ")}
        >
          About
        </a>
      </Link>
    </nav>
  );
```

​          

### 왜 사용할까 ?

css 클래스가 중첩되는 것을 방지할 수 있음

css module을 사용해서 스타일을 지정하면 class 명이 무작위 이름로 생성되는 것을 볼 수 있음

`class="NavBar_nav__OBiyO"` => 즉, 같은 이름의 class에 스타일을 지정해도 중첩되지 않음 !

​              

-------------

​             

## Styled JSX

니꼬쌤이 좋아하는 NextJS에서 styles 추가하는 방법

​            

### 사용법

jsx를 속성으로 갖는 style 태그를 component 본문에 작성

```css
<style jsx>{`
	nav {
		background-color: tomato;
	}
	a {
		text-decoration: none;
	}
`}</style>
```

styled JSX 또한 css module 처럼 class명이 무작위로 생성됨 -> css 클래스 중첩 방지, 모듈들이 독립적임

scoped가 적용되는 것

​           

------

​           

## Custom App

Next.js는 App component를 사용하여 page를 초기화함

이를 재정의하고 페이지 초기화를 제어할 수 있고, 이를 통해

1. 페이지 변경 간에 레이아웃 유지
2. 페이지 탐색 시 state 유지
3. componentDidCatch를 사용한 Custom 에러 처리
4. 페이지에 추가 데이터 삽입
5. Global CSS 추가

와 같은 일을 할 수 있음

​          

style 태그에 global 속성을 달아 global cs를 설정할 수 있지만, 페이지 이동 시 적용이 되지 않음

-> 페이지마다 같은 css 복붙 ? 비효율적인 방법

​              

### 사용법

기본 App를 재정의하려면 `./pages/_app.js` 파일 생성 -> NextJS는 이 파일을 자동으로 불러옴

생성 후, 아래와 같이 작성

```javascript
export default function App({ Component, pageProps }) {
  return (
    <div>
      <Component {...pageProps} />
    </div>
  );
}
```

​                      

------------

​                 

# #2 PRACTICE PROJECT

​                   

## Patterns

React 모델을 사용하면 페이지를 일련의 컴포넌트로 분해할 수 있음 

이러한 컴포넌트 중 많은 부분이 페이지 간에 재사용되는 경우가 많음 ( ex. NavBar , Footer )



### Layouts

```javascript
import NavBar from "./NavBar";

export default function Layout({ children }) {
  return (
    <>
      <NavBar />
      <div>{children}</div>
    </>
  );
}
```

​             

### Head

`import Head from "next/head";`

페이지 head에 element를 추가하기 위한 내장 컴포넌트를 노출함

​               

### 사용법

head를 복붙해서 호출하지 않기 위해 `./components/Seo.js` 생성 후, 아래와 같이 작성

```js
import Head from "next/head";

export default function Seo({ title }) {
  return (
    <Head>
      <title>{title} | Next Movies</title>
    </Head>
  );
}
```

컴포넌트에서 `<Seo title="About"></Seo>` 와 같이 사용하면 됨

​             

-------------------------

​              

## Fetching Data

> [The Movie DB](https://www.themoviedb.org/)

​             

------------

​           

## Redirect and Rewrite

​          

### Redirect 

들어오는 request 경로를 다른 destination 경로로 redirect할 수 있음

Redirect를 사용하려면 `next.config.js` 에서 redirects 키를 사용할 수 있음 

​          

`next.config.js` 내용

```js
module.exports = {
  reactStrictMode: true,
  async redirects() {
    return [
      {
        source: "/contact",
        destination: "/form",
        permanent: false,
      },
    ];
  },
};
```

Redirect는 source, destinatio 및 permanent 속성이 있는 객체를 포함하는 배열을 반환하는 비동기 함수

- source
  - 들어오는 request 경로 패턴 (request 경로)
- destination 
  - 라우팅하려는 경로 (redirect할 경로)
- permanent
  - true인 경우, 클라이언트와 search 엔진에 redirect를 영구적으로 cache하도록 지시하는 308 status code를 사용
  - false 인 경우, 일시적이고 cache되지 않음 307 status code를 사용

   => localhost:3000/contact를 치면 localhost:3000/form으로 redirect 됨!

​           

### Rewrite

들어오는 request 경로를 다른 destination 경로에 매핑할 수 있음

URL 프록시 역할을 하고 destination 경로를 mask하여 사용자가 사이트에서 위치를 변경하지 않은 것처럼 보이게 함 

Redirect는 반대로 새 페이지로 reroute되고 URL 변경사항을 표시함

-> user에게 API key를 숨길 수 있음

​          

`next.config.js` 

```js
async rewrites() {
    return [
      {
        source: "/api/movies",
        destination: `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`,
      },
    ];
  },
```

=> localhost:3000/api/movies 로 이동하면 뒤에 붙은 API_KEY는 가려져(mask)있음









