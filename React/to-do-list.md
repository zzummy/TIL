> NomadCoders - to do list 만들기
>
>  [to-do-list 작성 code 보기](https://github.com/zzummy/react-study/commit/a3e04ff38d23aff14f0eadf62e1ce3424278c311)

​                      

*toDo 목록을 담을 toDos에 toDo 목록을 추가하려면 ?* 

state를 직접 수정할 수 없음 => toDos.push() of toDo="" 로 수정할 수 없음

=> 함수를 가져와서 수정해야함

​         

#### ...currentArray를 이용해서 새로운 toDo를 기존의 배열에 추가하는 방법

`setToDos(currentArray => [toDo, ...currentArray])`

######           

```js
const [toDo, setToDo] = useState("");
  const [toDos, setToDos] = useState([]);

  const onChange = (event) => setToDo(event.target.value);
  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    setToDo("");
    setToDos(currentArray => [toDo, ...currentArray]);
  };
```

1. 비어있는 currentArray에 input을 통해 작성한 toDo("Hi")와 빈 array인 currentArray에 저장됨           
   ["Hi"]
2. 다음으로 입력하게 된 toDo("Bye")는 기존의 currentArray(["Hi"])에 추가가 됨           
   ["Bye", "Hi"]

​                

#### currentArray에 저장된 toDos들을 각 component로 만드는 방법

 `map()`은  배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환하며    
 첫번째 argument로 item을 가져올 수 있음

​                

`['one', 'two' ,'three', 'four'].map((item) => item.toUpperCase())`를 사용하면 item이 모두 대문자로 반환됨            
['ONE', 'TWO', 'THREE', 'FOUR']

​                

```html
<ul>
    toDos.map((item) => <li>{item}</li>)}
</ul>
```

​           

> **Warning: Each child in a list should have a unique "key" prop.**
>
> 같은 component의 list를 render할 때 key라는 prop을 넣어줘야 한다는 경고 발생

​           

map()함수를 보면 첫번째 argument에는 value, 두번째에는 index가 필요한 것을 볼 수 있음

`(method) Array<any>.map<JSX.Element>(callbackfn: (value: any, index: number, array: any[]) => JSX.Element, thisArg?: any): JSX.Element[]`

​          

따라서 고유한 key값을 설정해주면 됨

`{toDos.map((item, index) => <li key={index}>{item}</li>)}`

