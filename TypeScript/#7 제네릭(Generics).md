# #7 제네릭 (Generics)

​           

## 제네릭 ?

제네릭 이용하면 클래스나 함수, 인터페이스를 다양한 타입으로 재사용할 수 있음

​          

```typescript
function getSize(arr: number[] | string[]):number{
    return arr.length;
}

const arr1 = [1,2,3];
getSize(arr1);  // 3

const arr2 = ["a", "b", "c"];
getSize(arr2);  // 3 
```

매개변수 type이 바뀌었는데 동일한 함수를 사용하려면 앞서 배운 오버로드 혹은 Union을 사용

하지만 다른 type이 더 늘어나면 Union을 계속 사용하는 것은 좋지 않음

이럴 때 사용하는 것이 제네릭 ! 

​             

### 제네릭 사용

```typescript
function getSize<T>(arr: T[]):number{
    return arr.length;
}

const arr1 = [1,2,3];
getSize<number>(arr1);  // 3

const arr2 = ["a", "b", "c"];
getSize<string>(arr2);  // 3 

// type을 명시하지 않아도 자동으로 인식해줌
const arr3 = [false, true, true];
getSize(arr3);  // 3 

const arr4 = [{}, {}, {name: "Tim"}];
getSize(arr4);  // 3 
```

- 인터페이스에서 제네릭 사용

```typescript
interface Mobile<T>{
    name: string;
    price: number;
    option: T;
}

// const m1: Mobile<object> 도 가능
const m1: Mobile<{color: string; coupon: boolean}>={
    name: "s21",
    price: 1000,
    option: {
        color: "red",
        coupon: false,
    },
}

const m2: Mobile<string>={
    name: "s20",
    price: 900,
    option: "good",
}
```

```typescript
interface User{
    name: string;
    age: number;
}

interface Car {
    name: string;
    color: string;
}

interface Book{
    price: number;
}

const user:User = {name : "a", age: 10};
const car:Car = {name:"bmw", color: "red"};
const book: Book = {price: 3000};

// book에는 name이 없음
// T type은 name이 string인 객체를 확장한 형태라고 나타내는 것
function showName<T extends {name: string}>(data: T): string{
    return data.name;
}

showName(user);
showName(car);
// showName(book);
```

