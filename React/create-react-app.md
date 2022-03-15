# CREATE REACT APP



ReactJS를 사용하기 위해 더이상 <script> 태그로 import하지 않아도 됨

   

<https://nodejs.org/en> 에서 nodejs 설치 후, cmd에 `node -v` 확인

그리고 cmd에 `npx` 명령어 입력되면 준비 완료

  

만들어줄 폴더명(react-for-beginners)로 생성 `npx create-react-app react-for-beginners` 

  

package.json을 열어보면 실행시킬 수 있는 script들을 볼 수 있음 

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
```

`npm run start` or `npm start` 하면 development server를 만들게 됨

 

##### propTypes 설치

`npm i prop-types`  후, `import PropTypes from "prop-types";` 하고 사용해주면 됨

​    

----



컴포넌트 당 1개의 .js 파일 가질 수 있어 모듈화 가능

-> 스타일은 .module.css 파일을 생성 후, import하여 사용 

`import styled from "./Button.module.css";`

​      

**css 코드를 자바스크립트 객체로 변환시켜줘 css 코드에 작성된 btn이라는 클래스명을 property 접근 연산자(.)를 사용해서 이용가능**

`<button className={styled.btn}>{text}</button>`











