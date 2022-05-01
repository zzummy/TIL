# Recoil 

> [공식문서](https://recoiljs.org/ko/docs/introduction/core-concepts)

Recoil을 사용하면 *atoms* (공유 상태)에서 *selectors* (순수 함수)를 거쳐 React 컴포넌트로 내려가는 data-flow graph를 만들 수 있다.

​       

**Atoms** 컴포넌트가 구독할 수 있는 상태의 단위

**Selectors**는 atoms 상태값을 동기 또는 비동기 방식을 통해 변환함

​              

### Atoms

- 상태의 단위
- 업데이트와 구독 가능
- atoms이 업데이트되면 각각의 구독된 컴포넌트는 새로운 값을 반영하여 다시 렌더링 됨
- 런타임에서 생성될 수 있음
- 동일한 atom이 여러 컴포넌트에서 사용되는 경우 모든 컴포넌트는 상태를 공유

- `atom()`함수를 사용해 생성

  ```js
  const fontSizeState = atom({
      key: "fontSizeState",
      default: 14,
  })
  ```

- `useRecoilState`라는 훅을 이용해 컴포넌트에서 atom을 쓰고 읽음

  React의 `useState`와 비슷하지만 **상태가 컴포넌트 간 공유 가능**

  ```js
  function FontButton() {
    const [fontSize, setFontSize] = useRecoilState(fontSizeState);
    return (
      <button onClick={() => setFontSize((size) => size + 1)} style={{fontSize}}>
        Click to Enlarge
      </button>
    );
  }
  ```

  `fontSizeState`를 사용하는 다른 컴포넌트의 글꼴 크기도 같이 변함

​              

### Selectors

- atoms나 다른 selectors를 입력으로 받아들이는 순수 함수(pure function)

- 상위의 atoms 또는 selectors가 업데이트되면 하위의 selector 함수도 다시 실행됨

- 컴포넌트들은 selectors를 atoms처럼 구독 가능

- selectors가 변경되면 컴포넌트들도 다시 렌더링 됨

- 상태를 기반으로 하는 파생 데이터를 계산하는데 사용

  최소한의 상태 집합만 atoms에 저장

- 컴포넌트 관점에선 selectors와 atoms는 동일한 인터페이스를 가지므로 서로 대체 가능

- `selector`함수를 사용

  ```js
  const fontSizeLabelState = selector({
    key: 'fontSizeLabelState',
    get: ({get}) => {
      const fontSize = get(fontSizeState);
      const unit = 'px';
  
      return `${fontSize}${unit}`;
    },
  });
  ```

  `get` 속성 

  - 계산될 함수
  - 전달되는 `get`인자를 통해 atoms와 다른 selectors에 접근할 수 있음
  - 다른 atoms나 selectors에 접근하면 자동으로 종속 관계가 생성되므로, 참조했던 다른 atoms나 selectors가 업데이트되면 이 함수도 다시 실행됨

- `useRecoilValue()`를 사용해 읽을 수 있음

  하나의 atom이나 selector를 인자로 받아 대응하는 값 반환

​        