>  CREATE REACT APP



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





