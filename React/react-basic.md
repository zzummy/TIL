`React` 란 ? 

자바스크립트 라이브러리 하나인 웹 프레임워크로 UI를 만들기 위해 사용됨





React JS를 설치하기 위해서는 두 개의 Javascript 코드(readct, react-dom) import

React에서 html을 작성하지 않고 생성하려면 react-dom 사용해야함

react-dom은 모든 React element들을 HTML body에 둘 수 있도록 해줌



```html
<!DOCTYPE html>
<html>
  <body>
		<div id="root"></div>
	</body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
	<script>
		const root = document.getElementById("root");
		const span = React.createElement("span");
		ReactDOM.render(span, root)
	</script>
</html>
```

ReactDOM.render(span, root) 

​	React element를 가지고 HTML로 만들어 배치함 -> 사용자에게 보여줌

​	2가지 render하려면

​	`const container = React.createElement("div", null, [span,btn]) `

   `ReactDOM.render(container, root);`



```js
const h3 = React.createElement("h3", 
		{
			onMouseEnter: () => console.log("mouse enter")
		},
		"Hello Im a h3");
const btn = React.createElement("button", 
		{
			// event listener => property에 넣을 수 있음
			onClick:() => console.log("i'm clicked")
		}, "Click me");
```



------



createElement 대체 방법 -> JSX 

JSX 란, javascript를 확장한 문법으로 객체를 표현
\- HTML 이랑 문법 구조가 비슷
\- 리액트는 JSX문법을 사용, 그러나 브라우저는 JSX문법을 이해하지 못함
\- Babel을 사용하면 브라우저가 이해할 수 있도록 JSX 문법을 변환해줌
\- 스크립트 태그를 추가하고 리액트를 작성하고 있는 스크립트 태그에 type="text/babel"을 넣어줌



- JSX 사용

```js
// JSX 사용
const Title = (
	<h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
		Hello Im a title
	</h3>
);
const Button = (
     <button
       style={{
         backgroundColor: "tomato",
       }}
        onClick={() => console.log("im clicked")}
      >
        Click me
      </button>
   );
```



- Babel 

​	JSX를 브라우저가 이해할 수 있는 형태로 바꿔줌 

```html
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
```



- JSX를 사용한 createElement()

  ```js
  function Title() {
        return (
          <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
            Hello Im a title
          </h3>
        );
      }
  
      const Button = () => (
        <button
          style={{
            backgroundColor: "tomato",
          }}
          onClick={() => console.log("im clicked")}
        >
          Click me
        </button>
      );
  
      const Container = (
        // component 앞글자는 꼭 ! 대문자로 ! 소문자로 적으면 html로 인식
        <div>
          <Title />
          <Button />
        </div>
      );
  ```

- function으로 만들어 JSX return 하는 방식과 `= () => (` 는 같은 결과



