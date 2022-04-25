# styled-components

> [공식문서](https://styled-components.com/docs/basics)

​              

CSS-in-JS 라이브러리로, 

CSS-in-JS는 스타일 정의를 CSS 파일이 아닌 JS로 작성된 컴포넌트에 바로 삽입하는 스타일 기법

​            

## 설치

`npm i styled-components`

​          

## 기본 문법

styled-components 사용하기 위해서 `styled` 함수를 임포트

`import styled from "styled-components";` 

모든 알려진 HTML 태그에 대해서 이미 속성이 정의되어 있기 때문에 해당 태그명의 속성에 접근 가능

```js
import styled from "styled-components";

styled.button`
  // <button> HTML 엘리먼트에 대한 스타일 정의
`;
```

```js
import styled from "styled-components";
import Button from "./Button";

styled(Button)`
  // <Button> React 컴포넌트에 스타일 정의
`;
```

React 컴포넌트를 스타일링 할 때는 해당 컴포넌트를 임포트 후 인자로 해당 컴포넌트를 넘기면 됩니다.

​            

## 고정 스타일링

`styled` 함수가 리턴하는 건 React 컴포넌트이기 때문에 JSX를 통해 자유롭게 사용가능

```js
import styled from "styled-components";

const styledBtn = styled.button`
	color: white;
`;

export default function Home({children}){
    return(
    	<styledBtn>{children}</styledBtn>;
    )
}
```

