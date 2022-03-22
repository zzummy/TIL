> NomadCoders - ReactJS로 영화 웹 서비스 만들기



## 목차

[1. React란 ?](#react란?)               
	[JSX](#jsx)

[2. state란 ?](#state)   
	[React.useState()](#react.usestate)   
	[modifier는 왜 쓰나?](#modifier는-왜-쓰나-?)         
	[function에서 class로 변환하기](#function에서-class로-변환하기)             
	[Lifecycle(생명주기란)](#lifecycle)         
	[setState()](#setstate)

[3. unit conversion (단위 변환) 앱 만들기](#unit-conversion-단위-변환-앱-만들기)   
	[Minutes to Hours 변환](#minutes-to-hours-변환)    
	[Hours to Minutes 하는 flip function 만들기 ](#hours-to-minutes-하는-flip-function-만들기)   
	[select를 사용한 단위 변환기](#select를-사용한-단위-변환기)    

[4. props란 ?](#props)    
	[React.memo()](#react.memo)

[5. Effects](#effects)            
	[useEffect() ](#useeffect)          
	[Cleanup](#cleanup)

​            

​         

--------

​                

​              

[(+) 함수 정리](#함수-정리)     

- [fetch()](#fetch)
- [async-await](#async-await)

​                

​              

​               

## React란?

자바스크립트 라이브러리 하나인 웹 프레임워크로 UI를 만들기 위해 사용됨

​          

React JS를 설치하기 위해서는 두 개의 Javascript 코드(readct, react-dom) import    

React에서 html을 작성하지 않고 생성하려면 react-dom 사용해야함   

react-dom은 모든 React element들을 HTML body에 둘 수 있도록 해줌

​          

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

React element를 가지고 HTML로 만들어 배치함 -> 사용자에게 보여줌

2가지 render하려면

`const container = React.createElement("div", null, [span,btn]) `

`ReactDOM.render(container, root);`

​          

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

​          

------

​          

### JSX

createElement 대체 방법 

​          

**JSX** 란, javascript를 확장한 문법으로 객체를 표현    

- HTML 이랑 문법 구조가 비슷    
- 리액트는 JSX문법을 사용, 그러나 브라우저는 JSX문법을 이해하지 못함   
- Babel을 사용하면 브라우저가 이해할 수 있도록 JSX 문법을 변환해줌   
- 스크립트 태그를 추가하고 리액트를 작성하고 있는 스크립트 태그에 type="text/babel"을 넣어줌   

​          

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

​          

- Babel 

​	JSX를 브라우저가 이해할 수 있는 형태로 바꿔줌 

```html
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
```

​          

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

​          

## state

기본적으로 데이터가 저장되는 곳, 동적인 값

웹 애플리케이션을 렌더(render)하는데 있어 영향을 미칠 수 있는 값

​          

### React.useState()

> 변수, 변수를 제어하는 함수로 구성되며 변하는 값을 제어, 해당 부분의 리렌더링을 위함

​         

```js
const data = React.useState();
console.log(data);
```

log 출력 : (2) [undefined, f]    
undefined = data   
f = data 변경할 때 호출할 func                

​          

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

​          

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

​          

​           

### function에서 class로 변환하기

1. React.Component를 확장하는 동일한 이름의 ES6 class 생성
2. render() 메서드 추가
3. 함수 내용을 render() 메서드 안으로 옮김
4. render() 안에 있는 props를 this.props로 변경
5. 남아있는 빈 함수 선언 삭제



> 시계 예시 코드

``` js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

- `date`를 props에서 state로 이동
  - `this.props.state` -> `this.state.date`로 변경
  - 초기 `this.state`를 지정하는 class constructor 추가
  - <Clock /> 요소에서 `date` prop 삭제

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

​          

​                   

### Lifecycle

> 생명주기 메서드를 클래스에 추가하기



- **생명주기 메서드**

  - 마운팅 : component가 처음 DOM에 렌더링 될 때

  - 언마운팅 : component에 의해 생성된 DOM이 삭제될 때
  - `componentDidMount()` : component 출력물이 DOM에 렌더링 된 후에 실행
  - `componentWillUnmount()` : component가 DOM에서 삭제될 때 실행

​              

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

> 동작 원리

1. `<Clock />` 가 `ReactDOM.render()`로 전달되었을 때 `Clock` 컴포넌트의 constructor를 호출함         
   => `this.state`를 초기화
2. `<Clock />` 컴포넌트의 `render()` 메서드 호출함 (React는 화면에 표시될 내용을 알게 됨)          
   => Clock의 렌더링 출력값을 일치시키기 위해 DOM 업데이트
3. Clock 출력값이 DOM에 삽입되면, React는 `componentDidMount()` 호출             
   => 매초 컴포넌트의 `tick()`메서드 호출하기 위한 타이머를 설정하도록 브라우저에 요청함
4. 매초 브라우저가 `tick()`호출함. 그 안에서 Clock 컴포넌트는 `setState()`에 현재 시각을 포함하는 객체를 호출하면서 UI 업데이트 진행         
   => `setState()`호출로 state가 변경됨을 인지하기 위해 `render()` 다시 호출             
   => 이 때 `render()` 메서드 안의 `this.state.date`가 달라지고 렌더링 출력값은 업데이트된 시간을 포함       
   => DOM 업데이트
5. Clock 컴포넌트가 DOM으로부터 한 번이라도 삭제된 적이 있다면 타이머를 멈추기 위해 React는 `componentWillUnmount()` 생명주기 메서드 호출

​             

​               

### setState()

> state 올바르게 사용하기



#### ***1. state는 직접 수정할 수 없음 -> setState()를 사용하자***

- `this.state.comment = 'Hello';` 			불가능

- `this.setState({comment : 'Hello'});` 가능

- this.state를 지정할수 있는 곳은 `constructor`

​         

#### ***2. state 업데이트는 비동기적일 수 있음***

- React는 여러 `setState()` 호출을 한꺼번에 처리할수 있음

- `this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에 다음 state를 계산할 때 해당 값에 의존해서는 안됨

- ```js
  // Wrong
  this.setState({
    counter: this.state.counter + this.props.increment,
  });
  ```

- ```js
  // Correct
  // 이전 state를 첫 번째 인자로, 업데이트가 적용된 props를 두 번째 인자로 받음
  this.setState((state, props) => ({
    counter: state.counter + props.increment
  }));
  ```

- ```js
  // Correct
  this.setState(function(state, props) {
    return {
      counter: state.counter + props.increment
    };
  });
  ```

​      

#### ***3. state 업데이트는 병합됨***

- `setState()`를 호출할 때 React는 제공한 객체를 현재 state로 병합함

- 예를 들어, state는 다양한 독립적인 변수를 포함할 수 있음          

  ```js
   constructor(props) {
      super(props);
      this.state = {
        posts: [],
        comments: []
      };
    }
  ```

  ​               

​                

--------------

​          

## unit conversion (단위 변환) 앱 만들기

만들기 전에!     

JSX는 class / for 와 같은 JavaScript에서 선점된 문법 용어 사용할 수 없다.      

따라서 class는 `className` 으로, for은 `htmlFor` 로 바꿔서 작성한다.     

​          

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

​          

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

​          

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

​          

​          

-----

​          

​          

## props

> properties로, 부모 컴포넌트로부터 자식 컴포넌트로 data를 전송할 때 사용함    
>
> 태그의 속성 값을 함수의 argument처럼 컴포넌트에 값을 전달해줌
>
> props는 객체 형태로 전달

​            

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

​          

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

​          

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

​          

​          

### React.memo()

> 부모 컴포넌트에서 리렌더링되면, 모든 자식 컴포넌트들도 리렌더링 되는 경우를 막기 위해 사용됨
>
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

​          

`<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>`을 통해 PropTypes - props의 type을 알 수 있음

​          

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

​          

## EFFECTS

리렌더링 될 때마다 한 번만 받아도 될 정보 또한 계속 불려지는 문제가 발생함

코드의 규모가 클 경우 리렌더링이 계속되는 경우는 비효율적이기 때문에 이에 대한 방법을 알아보자. 

​          

*첫번째 render에만 코드가 실행 -> 다른 state 변화에는 실행되지 않도록*

예를들어 API통해 데이터 가져올 때, state가 변화할 때마다 render되지 않도록 코드 작성

​          

그래서 사용하는 ***useEffect*** 

​          

### useEffect

`useEffect(effect: EffectCallback, deps?: DependencyList)`

> 두 개의 argument를 가지는 function으로 코드의 실행 시점을 관리할 수 있는 선택권을 얻는 일종의 방어막
>
> 첫번째 argument : 우리가 딱 한번만 실행하고 싶은 코드
>
> 두번째 argument : [] 빈 값일 경우, 최초 1회 실행 / 있을 경우, 해당 값이 변할 때 실행됨 (여러개 입력 가능)



- useEffect 예시(1)

```js
function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);

  console.log("i run all the time");

  useEffect(() => {
    console.log("CALL THE API...");
  }, [])

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onClick}>click me</button>
    </div>
  );
}
```

useEffect에 들어간 함수는 state가 변경되도 실행되지 않는 것을 확인할 수 있음 ! 싱기

=> 우리 코드가 딱 한 번 실행될 수 있도록 보호해줌 



- useEffect 예시 (2)    
  *search keyword에 변화가 있을 때만* API 호출하고 싶을 때 (다른 버튼 클릭시 search API 호출X)

```js
const [keyword, setKeyword] = useState("");
  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);
  
useEffect(() => {
    if (keyword !== "" && keyword.length > 5) {
      console.log("SEARCH FOR", keyword);
    }
  }, [keyword]);
```

똑같이 useEffect()를 사용하면 딱 한 번만 SEARCH FOR 로그가 출력됨

=> 두번째 argument에 keyword를 적으면, `keyword`가 변할 때 코드 실행됨       
(react에게 keyword만 지켜봐 ! 라는 것)

=> 조건문을 넣어줄 수도 있음

=> [keyword, counter]처럼 2개도 가능

​           

​             

### Cleanup

> 컴포넌트가 destroy될 때 실행될 함수



``` js
function Hello() {
  useEffect(() => {
    console.log("created :)");
    return () => console.log("destroyed :(")
  }, []);

  return <h1>Hello</h1>;
}
```

```js
function Hello() {
  function byFn() {
    console.log("bye :(");
  }
  function huFn() {
    console.log("created :)");
    return byFn;
  }
  useEffect(huFn, []);

  return <h1>Hello</h1>;
}
```

둘 다 같은 방법 



​             

​             

​             

​             

​             

---------------

​             

## 함수 정리

​             

### fetch()

> Request나 Response와 같은 HTTP 파이프라인을 구성하는 요소를 조작할 수 있게 해주는 API
>
> 비동기 네트워크 통신을 보다 더 쉽게 기술할 수 있음
>
> JavaScript 내장 Web API (Axios는 Promise 기반 HTTP 클라이언트)

​              

- 기본 문법

  ```js
  fetch('api 주소')
  	.then(res => res.json())
  	.then(res => {
  		//data를 응답 받은 후의 로직
  	});
  ```

  ​                

- 3개의 interface

  - **Headers** : HTTP header와 대응되는 객체

  - **Request** : HTTP 요청을 통해 자원을 가져오는 인터페이스 

    - URL, Header, Body가 필요

    - Request에 대한 mode 제한과 certidicate 관련 설정도 추가 가능

    - Request 객체의 첫번째 인자는 호출한 Path, 두번째 인자는 Request에 대한 정보(method, header, body 등등) 들어감

      - **method** 는 HTTP method와 동일한 스팩으로 들어가면 됨 
        
        GET/POST/HEAD/PUT/DELETE/OPTION/PATH 등등        
        
        `GET(defualt)` : 어떤 정보 가져올 때        

        `POST` : 어떤 정보를 backend로 보낼 때        
        
        `DELETE` : 어떤 정보를 삭제할 때        
        
      - **headers** 는 Request와 Header를 지정해주는 곳        
        
        *Object lireral*과 *Headers 객체의 인스턴스*를 넣을 수 있음        
        
        ```js
        const request = new Request('/api/posts',{
        	method: "GET",
        	headers:{
        		'content-type' : 'application/json',
        	}
        });
        
        const request2 = new Request('/api/posts'),{
        	method: 'GET',
        	header: new Headers({
        		'content-type' : 'application/json',
        	})
        })
        ```
        
      - **body** 는 HTTP Request에 실을 데이터인데 여러가지 타입을 넣을 수 있음        
        
        즉, *전달하고자 하는 응답 내용**
        
        ***객체 타입**으로 작성해야 함
  
  - **Response** : fetch를 호출하면 가져올 수 있는 객체
  
    - status : HTTP response code를 담고 있음 (성공 : 200)
    - statusText : 기본값 ok. 상황에 따라 다른 message
    - ok : status의 200-299의 값을 추상화한 boolean (200-299 : true)
    - headers : Response headers이기 때문에 headers의 guard 속성은 response
    - type : response객체의 type

​                

- **res.json()의 의미**

  첫번째 then함수에 전달된 인자 res는 HTTP 통신 요청과 응답에서 응답의 정보를 담고있는 객체    
  하지만 콘솔 창에서 확인해보면 실제 data는 찍히지 않음                 

  응답으로 받는 JSON 데이터를 사용하기 위해서 Response Object의 json함수를 호출하고 return 해야함    

  -> return된 값이 두번째 then 함수의 인자로 옴 (Object 형태로)



```js
useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers")
      .then((response) => response.json())
      .then((json) => {
        setCoins(json);
        setLoading(false);
      });
  }, [])
```

[fetch 사용 코드](https://github.com/zzummy/react-study/commit/222c56d9c15188720423d1da5cc3afea6b7083ba#diff-a57feca475bb6559f612489180521851c3b213aaa4a38961899f184137d8d7f0L25 )

​                 

​               

### async-await

> Promise의 불편한 점 개선 (참고 : https://www.daleseo.com/js-async-async-await/)
> then보다 보편적으로 많이 사용됨
>
> 비동기 코드를 마치 동기 코드처럼 보이게 작성

​           

``` js
const getMovies = async () => {
    const response = await (fetch(
      `https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year`
    ));
    const json = await response.json();
    setMovies(json.data.movies);
    setLoading(false);
  }
```

​              



