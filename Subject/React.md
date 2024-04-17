## Hooks

![[hook-flow.png|500]]

1. [[useState]]
2. [[useEffect]]


## Rules of React
1. [[Components and Hooks must be pure]]
2. [[React calls Components and Hooks]]
3. [[Rules of Hooks]]

## Learn React
### Describing the UI
1. [[Keeping Components Pure]]

### Escape Hatches
1. [[Referencing Values with Refs]]
	컴포넌트가 정보를 "기억하기" 원할 때, 하지만 그 정보가 새로운 [render를 트리거](https://react.dev/learn/render-and-commit) 시키지 않기를 원하는 경우, ref를 사용할 수 있다.

```jsx
const ref = useRef(0);
```

2. [[Manipulating the DOM with refs]]
	React는 자동으로 render 출력에 맞게 DOM을 업데이트 한다. 그래서 컴포넌트는 그것을 제어하는 것을 필요로 하지 않는다. 하지만 때때로 React에 의해 관리되는 DOM 요소에 접근할 필요가 있을 수 있다. - 예를들어, **node를 포커스**하기 위해, 그것을 **스크롤** 하기 위해, 그것의 **사이즈나 위치를 측정**할 필요가 있을 수 있다. React에서 그것을 하기 위한 내장된 방법은 없다. 그렇기에 DOM node에 ref가 필요할 것이다. 예를들어 버튼을 클릭하는 것은 ref를 사용하여 input을 focus할 것이다:

```jsx
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
	inputRef.current.focus();
  }

  return (
	<>
	  <input ref={inputRef} />
	  <button onClick={handleClick}>
		Focus the input
	  </button>
	</>
  );
}	
```

3. [[Synchronizing with Effects]]
	일부 컴포넌트는 외부 시스템에 동기화할 필요가 있다. 예를들어, React 상태를 기반으로 non-React 컴포넌트를 제어하거나 서버 연결을 설정하거나 혹은 컴포넌트가 화면에 나타날 때 분석 로그를 보내는 것을 원할 수 있다. 특별한 이벤트를 다룰 수 있게 해주는 event handlers와 달리 Effects는 랜더링 이후에 코드를 실행할 수 있게 해준다. 그것을 이용하여 React의 외부 시스템에 컴포넌트를 동기화할 수 있다. 

4. [[You Might Not Need An Effect]]
	Effect는 React 패러다임으로부터의 탈출구이다. 그들은 React의 "밖으로 나갈 수 있게" 해주고 일부 외부 시스템에 컴포넌트를 동기화할 수 있게 해준다. 만약 포함된 외부 시스템(예를들어, 일부 props나 state가 변화되었을 때 컴포넌트의 상태를 업데이트 하기 원하는 경우)이 없다면, Effect가 필요하지 않을지도 모른다. 불필요한 Effect를 제거하는 것은 따라가기 쉽고 빠르게 실행되며 덜 오류가 발생하는 코드를 만들 것이다.

	Effect가 불필요한 흔한 두 가지 경우가 있다:
	- **랜더링에 데이터를 변경하는 Effect는 필요하지 않다.** 
	- **유저 이벤트를 다루는 Effect는 필요하지 않다.**

5. [[Lifecycle of reactive effects]]
	Effect는 컴포넌트로부터 다른 lifecycle을 가진다. 컴포넌트는 mount, update 혹은 unmount 된다. Effect는 오직 두 가지를 가질 수 있다: 무언가 동기화를 시작하고 이후에 그 동기화를 멈춘다. 이것은 만약 Effect가 수 번 변하는 props 그리고 state에 의존하고 있는 경우 여러번 발생할 수 있다.

6. [[Separating events from Effects]]

> [!caution] Under Construction
> 이 section은 React의 안정된 버전에 아직 나오지 않은 실험적인 API를 다루고 있다.

7. [[Removing Effect dependencies]]
	Effect를 작성할 때, 린터는 (props나 state 같은) Effect의 종속성(dependencies) 리스트에 있는 모든 reactive value를 포함했다는 것을 보장한다. 이것은 Effect가 컴포넌트에 최신의 props 그리고 state를 가지고 동기화를 유지화 하는 것을 보장한다. 불필요한 종속성은 Effect를 너무 자주 발생시키거나 심지어 무한 루프를 만들 수 있다.

8. [[Reusing logic with custom Hooks]]
	React는 `useState`, `useContext` 그리고 `useEffect`와 같은 내장 Hooks를 가지고 있다. 때때로, 좀 더 특정한 목적을 위한 Hook을 원할 수 있다: 예를 들어, 데이터를 패치하기 위해, 유저가 온라인인지 아닌지 체크하기 위해, 채팅방에 연결하기 위해 등. 이것을 위해, 어플리케이션에 필요한 Hooks을 만들 수 있다

## Post
1. [[컴포넌트와 순수성]]
2. [[서버 컴포넌트]]
3. [[React의 랜더링 프로세스를 통해 알아보는 리액트의 가치]]
