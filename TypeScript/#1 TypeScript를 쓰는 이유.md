# #1 TypeScript를 쓰는 이유

​            

## TypeScript를 쓰는 이유

우리가 사용하는 브라우저들은 TypeScript를 이해하지 못하기 때문에, JavaScript로 변환해서 로드해야 진행할 수 있음

​           

그런데도 TypeScript를 쓰는 이유는 뭘까 ?

​            

### JavaScript의 단점

1.  실수가 분명한 코드임에도 불구하고 문제없이 실행됨

   ```js
   function add (num1, num2){
       console.log(num1+ num2);
   }
   
   add();		// NaN
   add(1);		// NaN	(1+undefined)
   add(1,2);		// 3
   add(3,4,5);		// 7
   add('hello', 'world');		// "helloworld"
   ```

2.  JavaScript는 동적인 언어

   - 런타입에 타입이 결정되고, 오류가 발견됨

   - 개발자의 실수를 사용자가 고스란히 발견함

   ```js
   function showItems(arr){
       arr.forEach((item) => {
           console.log(item);
       });
   }
   
   showItems([1,2,3]);		// 1 2 3 정상 출력
   showItems(1,2,3);		// Uncaught TypeError 
   ```

   ​               

   반면, Java, TypeScript와 같은 정적 타입 언어는 컴파일 시, TypeError가 발견됨 

   => *안정적이고 빠른 작업을 진행할 수 있는 장점이 있음*

​                

### TypeScript로 바꿔보자

#### 함수의 parameter에 type을 명시해줄 때 

1. 다른 개발자가 작성한 함수를 사용할 때에 어떤 type으로 전달해줘야할지 일일히 코드를 찾아볼 필요가 없음
2. 사전에 오류를 방지할 수 있음 

```typescript
// parameter인 num1, num2가 'any' type
// type을 명시해주자 ! 
function add (num1:number, num2:number){
    console.log(num1+ num2);
}

// 함수에 정의한 parameter 개수가 맞지 않으면 에러 발생
// add();		
// add(1);		
// add(3,4,5);		

add(1,2);		

// add 함수에 정의한 parameter의 type이 맞지 않으면 에러 발생
// add('hello', 'world');		
```

​         

```typescript
function showItems(arr:number[]){
    arr.forEach((item) => {
        console.log(item);
    });
}

showItems([1,2,3]);		
// showItems(1,2,3);	제대로 된 사용법을 지키지 않았음을 알려줌	
```



