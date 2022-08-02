# [Java] 입력 Scanner와 BufferedReader

​        

## 1. Scanner

[java.lang.Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)
	[java.util.Scanner](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)



#### Scanner의 특징

1. 입력 받을 때 정수, 소수, 문자 데이터를 구분해서 읽어들일 수 있다.

   문자열(string) 입력시, next() 나 nextLine()
   정수(int) 입력시, nextInt()

   그외로 nextDouble(), nextFloat(), nextBoolean()과 같이 사용할 수 있다.

   추가적으로 문자(char)는 문자열로 입력 받은 후, chatAt() 메소드를 통해 문자로 반환해야한다.

   

2. 공백(띄어쓰기)와 개행('','\t','\r','\n' 등)을 기준으로 입력받을 수 있다.

​       

#### Scanner 예시

[백준_2558_A+B-2](https://www.acmicpc.net/problem/2558)

```java
import java.util.Scanner;       // Scanner Class 호출

public class Main_BOJ_2558 {
    public static void main(String[] args) {

        // Scanner 객체 생성
        Scanner sc = new Scanner(System.in);    // System.in : 사용자로부터 입력받는 입력 스트림

        int a = sc.nextInt();       // int형 입력 및 return
        int b = sc.nextInt();

        System.out.println(a+b);

    }
}
```

​               

#### + 추가

**next()와 nextLine() 차이**

둘 다 문자열(string)을 입력받을 때 쓰는 메소드이지만 차이점이 있다.

next()는 공백 입력 전까지의 문자열만을 반환한다.

nextLine()은 enter 입력 전까지의 모든 문자열을 반환한다.

​        



## 2. BufferedReader

[java.lang.Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)
	[java.io.Reader](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html)
		[java.io.BufferedReader](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)

​          

#### BufferedReader의 특징

1. 버퍼가 있는 스트림이다.

   개행이 입력되거나 버퍼가 꽉 차게되면 버퍼를 비우면서 입력된 값을 한 번에 전달하게 된다.

2. 별다른 정규식을 검사하지 않는다.

   Scanner와 다르게 문자열 그대로 읽어 들여 별다른 정규식을 검사하지 않는다.   

3. 입력받은 값이 문자열(string)으로 고정되기 때문에, 데이터를 가공해서 사용해야 한다.

   BufferedReader의 입력은 **readLine()** 메소드를 사용하는데, 리턴값이 문자열로 고정되어 있기 때문에, 형변환이 필요하다.

   readLine()을 사용할 때, 예외처리가 꼭 필요하다. (`throws IOException`)

   데이터 가공을 위해 사용하는 것은 StringTokenizer와 String.split() 함수가 있다.

   **StringTokenizer**의 nextToken() 함수를 사용하면 readLine()을 통해 입력 받은 값을 공백 단위로 구분하여 순서대로 호출할 수 있다. 

   ++ 추후에 추가 예정 !

​           

#### BufferedReader 예시

위와 동일한 문제

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main_BOJ_2558 {
    public static void main(String[] args) throws IOException {

        // BufferedReader 객체 생성
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int a = Integer.parseInt(br.readLine());
        int b = Integer.parseInt(br.readLine());

        System.out.println(a+b);
    }
}
```





​     

결과적으로 

위는 bufferedReader, 아래는 Scanner 사용 시 걸린 시간이다. 

간단한 문제지만 시간이 꽤 차이나는 것을 볼 수 있었다. 

![화면 캡처 2022-08-02 195235](https://user-images.githubusercontent.com/62216158/182357886-5217b492-b723-47c9-991b-5deda71af383.png)