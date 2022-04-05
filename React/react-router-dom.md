## react-router-dom

React를 사용할 때 페이지 이동을 위한 라이브러리

[react-router-dom 공식문서](https://reactrouter.com/docs/en/v6/api)

​           

### 설치

`npm i react-router-dom`

​         

### 사용

```js
import { Link } from "react-router-dom"
```



### API 

- [useNavigate](#usenavigate)

- [useLocation](#uselocation)

​           

​               

------

​           

#### useNavigate

onClick event 실행 시, state 전달하기 위해 구글링을 통해서 useHistory을 찾았지만 react-router-dom v6에서 사라졌음 => 이를 대신한 **useNavigate**

​              

`useNavigate` : 특정 event가 발생할 때 해당 url로 이동할 수 있게 만들어줌 

`useNavigate(이동할 url, 전달할 인자)`

​            

```js
import {useNavigate} from "react-router-dom"

function Cart(){
    const handleClick = () => {
    	navigate(`/order`, state : {orderList: orderList});
	}

	return(
    	<img src="" alt="" onClick={handleClick} />
    );
}
```

이렇게 order 페이지로 이동하면서 state값을 전달할 수 있다.

​                      

​                      

#### useLocation

`useLocation` : useNavigate로 전달 받은 state를 사용하기 위함

​                      

```js
import {useLocation} from "react-router-dom"

function Order(){
    const location = useLocation();
    const { orderList } = location.state;

	return(
        (...)
    	 {orderList && orderList.map((item, index) => (
                    (...)
         ))}
		(...)
    );
}
```

state로 넘긴 인자를 받아서 사용할 수 있음





