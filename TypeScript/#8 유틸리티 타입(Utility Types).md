# #8 유틸리티 타입(Utility Types)



```typescript
interface User{
    id:number;
    name: string;
    age: number;
    gender: "m" | "f";
}

type UserKey = keyof User;  // 'id' | 'name' | 'age' | 'gender'

const uk:UserKey = "name";
```

여기서 **keyof**는 Object의 key들의 리터럴 값들을 가져옴

​            

### `Partial<T>`

Partial은 속성을 모두 optional로 바꿔줌 

그래서 일부 사용이 가능함

```typescript
interface User{
    id:number;
    name: string;
    age: number;
    gender: "m" | "f";
}

// interface User{
//     id?:number;
//     name?: string;
//     age?: number;
//     gender?: "m" | "f";
// }
// 와 같음

let admin:Partial<User> = {
    id: 1,
    name: "Bob",
}
```

​          

### `Required<T>`

모든 속성을 필수로 바꿔줌

```typescript
interface User{
    id:number;
    name: string;
    age?: number;
}

let admin:Required<User> = {
    id: 1,
    name: "Bob",
    age: 30,
}
```

​          

### `Readonly<T>`

읽기 전용으로 바꿈

즉, 처음 할당만 가능하고 이후 수정은 불가능함

```typescript
interface User{
    id:number;
    name: string;
    age?: number;
}

let admin:Readonly<User> = {
    id: 1,
    name: "Bob",
}

// admin.id = 4;
```

​                

### `Record<K,T>`

K : key, T : type

```typescript
// interface Score {
//     "1" : "A" | "B" | "C" | "D";
//     "2" : "A" | "B" | "C" | "D";
//     "3" : "A" | "B" | "C" | "D";
//     "4" : "A" | "B" | "C" | "D";
// }

type Grade = "1" | "2" | "3" | "4";
type Score = "A" | "B" | "C" | "D";

const score:Record<Grade, Score> = {
    1: "A",
    2: "B",
    3: "C",
    4: "D"
}
```

```typescript
interface User{
    id: number;
    name: string;
    age: number;
}

function isValid(user:User){
    const result: Record<keyof User, boolean> = {
        id: user.id > 0,
        name: user.name !== '',
        age: user.age > 0
    }

    return result;
}
```

​             

### `Pick<T,K>`

T type에서 K 속성만 골라서 사용함

```typescript
interface User{
    id: number;
    name: string;
    age: number;
    gender: "M" | "W";
}

const admin: Pick<User, "id" | "name"> = {
    id: 0,
    name: "Bob",
}
```

​              

### `Omit<T,K>`

Pick과 반대로 특정 속성을 생략하고 사용할 수 있음

```typescript
interface User{
    id: number;
    name: string;
    age: number;
    gender: "M" | "W";
}

const admin: Omit<User, "age" | "gender"> = {
    id: 0,
    name: "Bob",
}
```

​               

### `Exclude<T1,T2>`

T1에서 T2를 제외하고 사용하는 방식

```typescript
type T1 = string | number | boolean;
type T2 = Exclude<T1, number | string>;
// T2에는 boolean만 남음
```

​               

### `NonNullable<Type>`

null을 제외한 type을 생성 (undefined도 함께 제외시킴)

```typescript
type T1 = string | null | undefined | void;
type T2 = NonNullable<T1>;
// T2에는 string만 남음
```

​               

+) 더 많은 유틸리티 타입이 있으니 [공식문서](https://www.typescriptlang.org/docs/handbook/utility-types.html) 참고해서 공부하기 !

