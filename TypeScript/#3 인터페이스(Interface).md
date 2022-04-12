# #3 인터페이스(Interface)

​         

### 인터페이스 사용 이유  

객체는 object type으로 정의할 수 있음

```typescript
let user:object;

user = {
    name: 'xx',
    age: 30
}

// console.log(user.name);		Property 'name' does not exist on type 'object'
```

object에는 특정 속성 값에 대한 정보가 없으므로 에러

##### => property를 정의해서 객체를 표현하고자 할 때  Interface 사용

​           

### 인터페이스를 사용해보자

```typescript
type Score = 'A' | 'B' | 'C' | 'F';
// Score라는 type 생성해서, 이외의 string 값은 받을 수 없도록 설정 

interface User{
    name : string;
    age : number;
    gender? : string;    // 입력을 해도되고 안해도 되는 선택적인 속성
    readonly birthYear : number;
    [grade:number] : Score;    
}

// interface에 정의된 속성은 모두 작성해줘야 함
let user : User = {
    name : 'xx',
    age : 30,
    birthYear : 2000,
    1 : 'A',
    2 : 'B',
}

user.age = 10;
user.gender = "male";
// user.birthYear = 1990;

console.log(user.name);
```

- 옵션 속성
  - 입력을 해도 되고 안해도 되는 선택적인 속성값을 표현할 때
  - `gender? : string` 처럼 작성

- 읽기 전용 속성
  - 읽기 전용 속성이므로 수정할 수 없음
  - 최초 생성할 때만 할당 가능, 그 이후엔 수정할 수 없음
  - `readonly birthYear : number`

- 문자열 인덱스 서명 추가
  - `[grade:number] : string `
  - number를 key로 지정하고, string을 value로 갖는 property를 여러 개 가질 수 있다는 의미
- 문자열 Literal Type
  - 문자열과 숫자, boolean 세 가지 literal type으로 문자열이나 숫자에 정확한 값을 지정할 수 있음
  - `type Score = 'A' | 'B' | 'C' | 'F';`

​             

​          

### Interface로 함수 지정하기

`함수 형태` 

(함수로 전달되는 매개변수) : 리턴 타입

```typescript
interface Add {
    (num1:number, num2:number) : number;
}

const add : Add = function(x, y){
    return x + y;
}

add(10,20);
```

```typescript
interface IsAdult{
    (age:number) : boolean;
}

const a:IsAdult = (age) => {
    return age > 19;
}

a(20);
```

​              

### Interface로 클래스 정의하기

```typescript
interface Car {
    color : string;
    wheels : number;
    start() : void;
}

class Bmw implements Car{
    color = "red";
    wheels = 4;
    constructor(c:string){
        this.color = c;
    }
    start() {
        console.log('go...');
    }
}

const b = new Bmw('green');
console.log(b);
b.start();
```

​          

### Interface 확장하기

`extends` 사용

```typescript
// Car가 갖고 있던 속성 그대로 받음
interface Benz extends Car{
    door : number;
    stop() : void;
}

const benz : Benz = {
    door : 5,
    stop() {
        console.log('stop');
    },
    color : "blue",
    wheels : 4,
    start() {
        console.log('go...');
    }
}
```

-> 마찬가지로 Car가 갖고 있던 속성 그대로 작성해줘야 함

​          

#### 확장 여러 개 받기

``` typescript
interface Car {
    color : string;
    wheels : number;
    start() : void;
}

interface Toy{
    name : string;
}

interface ToyCar extends Car, Toy{
    price : number;
}
```



