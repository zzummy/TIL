> NomadCoders - ReactJS로 영화 웹 서비스 만들기 



## 목차

[1. React란 ?](#react란?)

[2. state란 ?](#state)   
	[React.useState()](#react.usestate())   
	[modifier는 왜 쓰나?](#modifier는-왜-쓰나-?)       

[3. unit conversion (단위 변환) 앱 만들기](#unit-conversion-(단위-변환)-앱-만들기)   
	[Minutes to Hours 변환](#minutes-to-hours-변환)    
	[Hours to Minutes 하는 flip function 만들기 ](#hours-to-minutes-하는-flip-function-만들기)   
	[select를 사용한 단위 변환기](#select를-사용한-단위-변환기)    

[4. props란 ?](#props)    
	[React.memo()](#react.memo())





## React란?

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



## state

기본적으로 데이터가 저장되는 곳, 동적인 값



### React.useState()

```js
const data = React.useState();
console.log(data);
```

log 출력 : (2) [undefined, f]    
undefined = data   
f = data 변경할 때 호출할 func                



**`const[상태 값 저장 변수, 상태 값 갱신 함수] = useState(상태 초기 값);`**

```js
function App(){
      let [counter, modifier] = React.useState(0);

      return(
        <div>
          <h3>
            Total clicks: {counter}
          </h3>
          <button>
            Click me
          </button>
        </div>
      );
}
```



### modifier는 왜 쓰나 ?

`let [counter, modifier] = React.useState(0);` 의 **modifier**(원하는 함수명으로 변경)은 state의 data 값 변경할 때 사용됨

-> 리렌더링

```js
function App(){
      let [counter, setCounter] = React.useState(0);
      const onClick = () => {
        // setCounter(counter + 1);
        setCounter((current) => current + 1);
      }
      return(
        <div>
          <h3>
            Total clicks: {counter}
          </h3>
          <button onClick={onClick}>
            Click me
          </button>
        </div>
      );
}
```

> setCounter((current) => current + 1) 는 값이 변경될 때마다 새로운 state로 current가 생성되기 때문에 더 안전함



--------------



## unit conversion (단위 변환) 앱 만들기

만들기 전에!     

JSX는 class / for 와 같은 JavaScript에서 선점된 문법 용어 사용할 수 없다.      

따라서 class는 `className` 으로, for은 `htmlFor` 로 바꿔서 작성한다.     



### Minutes to Hours 변환

```js
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
  </body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    function App() {
      const [minutes, setMinutes] = React.useState("");

      const onChange = (event) => {
        setMinutes(event.target.value);
      };

      const reset = () => setMinutes(0);

      return (
        <div>
          <h1 className="hi">Super Converter</h1>
          <div>
            <label htmlFor="minutes">Minutes</label>
            <input
              value={minutes}
              id="minutes"
              placeholder="Minutes"
              type="number"
              onChange={onChange}
            />
          </div>
          <div>
            <label htmlFor="hours">Hours</label>
            <input
              value={Math.round(minutes / 60)}
              id="hours"
              placeholder="hours"
              type="number"
            />
          </div>
          <button onClick={reset}>reset</button>
        </div>
      );
    }

    const root = document.getElementById("root");
    ReactDOM.render(<App />, root);
  </script>
</html>

```



### Hours to Minutes 하는 flip function 만들기

```js
function App() {
      const [amount, setAmount] = React.useState(0);
      const [flipped, setFlipped] = React.useState(false);

      const onChange = (event) => {
        setAmount(event.target.value);
      };

      const reset = () => setAmount(0);
      const onFlip = () => {
        reset();
        setFlipped((current) => !current);
      };
      
      return (
        <div>
          <h1 className="hi">Super Converter</h1>
          <div>
            <label htmlFor="minutes">Minutes</label>
            <input
              value={flipped ? amount * 60 : amount}
              id="minutes"
              placeholder="Minutes"
              type="number"
              onChange={onChange}
              disabled={flipped}
            />
          </div>
          <div>
            <label htmlFor="hours">Hours</label>
            <input
              value={flipped ? amount : Math.round(amount / 60)}
              id="hours"
              placeholder="hours"
              type="number"
              onChange={onChange}
              disabled={!flipped}
            />
          </div>
          <button onClick={reset}>reset</button>
          <button onClick={onFlip}>Flip</button>
        </div>
      );
    }

    const root = document.getElementById("root");
    ReactDOM.render(<App />, root);
  </script>
```



### select를 사용한 단위 변환기 

```js
function App() {
      const [index, setIndex] = React.useState("xx");
      const onSelect = (event) => {
        setIndex(event.target.value);
      };
      return (
        <div>
          <h1 className="hi">Super Converter</h1>
          <select value={index} onChange={onSelect}>
            <option value="xx">Select your units</option>
            <option value="0">Minutes & Hours</option>
            <option value="1">Km & Miles</option>
          </select>
          <hr />
          {index === "xx" ? "Please select your units" : null}
          {index === "0" ? <MinutesToHours /> : null}
          {index === "1" ? <KmToMiles /> : null}
        </div>
      );
    }
```

- 리렌더링 조건
  1. props 바뀔 때
  2. state 바뀔 때
  3. 부모 컴포넌트가 리렌더링 될 때





-----





## props

properties로, 부모 컴포넌트로부터 자식 컴포넌트로 data를 전송할 때 사용함    

props는 객체 형태로 전달

```js
function Btn(props) {
      console.log(props);
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px",
            border: 0,
            borderRadius: 10,
          }}
        >
          {props.text}
        </button>
      );
    }

    function App() {
      return (
        <div>
          <Btn text="Save Changes" />
          <Btn text="Continue" />
        </div>
      );
    }
```

Btn 컴포넌트에서 props.text로 접근할 수 있음



실제로 props.property로 사용하지 않고 {property}처럼 사용할 수 있음

```js
function Btn({ text, big }) {
      return (
        <button
          style={{
            backgroundColor: "tomato",
            color: "white",
            padding: "10px 20px",
            border: 0,
            borderRadius: 10,
            fontSize: big ? 18 : 16,
          }}
        >
          {text}
        </button>
      );
    }

    function App() {
      return (
        <div>
          <Btn text="Save Changes" big="false" />
          <Btn text="Continue" />
        </div>
      );
    }
```



`<Btn text={value} onClick={changeValue} />` 에서 onClick은 button 태그를 위한 이벤트리스너가 아님 ! 

props의 property일 뿐, button에서 메소드로 달아줘야함 

```js
function Btn({ text, changeValue }) {
      return (
        <div>
          <button
            onClick={changeValue}
            style={{
              backgroundColor: "tomato",
              color: "white",
              padding: "10px 20px",
              border: 0,
              borderRadius: 10,
            }}
          >
            {text}
          </button>
        </div>
      );
    }

    function App() {
      const [value, setValue] = React.useState("Save Changes");
      const changeValue = () => setValue("Revert Changes");
      return (
        <div>
          <Btn text={value} changeValue={changeValue} />
          <Btn text="Continue" />
        </div>
      );
    }

```





### React.memo()

> component의 props가 변경되지 않았을 때, 리렌더링을 방지하기 위해 사용되는 함수



```js
const MemorizedBtn = React.memo(Btn);

    function App() {
      const [value, setValue] = React.useState("Save Changes");
      const changeValue = () => setValue("Revert Changes");
      return (
        <div>
          <MemorizedBtn text={value} changeValue={changeValue} />
          <MemorizedBtn text="Continue" />
        </div>
      );
    }
```

Save Changes 버튼 클릭했을 때, Continue의 state는 변경되지 않았으므로 memo를 써주면 됨



`<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>`을 통해 PropTypes - props의 type을 알 수 있음



```js
Btn.propTypes = {
      text: PropTypes.string.isRequired,
      fontSize: PropTypes.number,
    };

    function App() {
      return (
        <div>
          <Btn text="Save Changes" fontSize={18} />
          <Btn text="14" fontSize={"Continue"} />
        </div>
      );
    }

```

를 사용하면 fontSize에 잘못된 type이 전송된 것을 console에서 확인할 수 있음

`PropTypes.string.isRequired` : isRequired는 필수 props임을 알려줌 





----





