`useState`는 컴포넌트에 [상태 변수](https://react.dev/learn/state-a-components-memory)를 추가할 수 있는 React Hooks이다.

```jsx
const [state, setState] = useState(initalState);
```

## Reference

### `useState(initialState)`

```jsx
import { useState } from 'react';  

function MyComponent() {  
const [age, setAge] = useState(28);  
const [name, setName] = useState('Taylor');  
const [todos, setTodos] = useState(() => createTodos());  
// ...
}
```

> [array destructuring](https://javascript.info/destructuring-assignment)을 이용하여 `[something, setSomething]`과 같이 상태 변수를 선언한다.

#### Parameters
- `initialState`:  초기에 원하는 상태 변수 값. 이 초기값은 초기에 랜더링 이후 무시된다.

> [!info] initialState에 함수를 전달하는 경우
> 이것은 `initializer` 함수로 다뤄진다. 이 함수는 순수 해야하며, 인자를 가져서는 안되고 타입을 가진 변수를 반환해야한다. React는 `initializer` 함수를 컴포넌트를 초기화할 때 호출하고, 초기 상태에 반환되는 변수를 저장한다. ([예시](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state))

#### Returns
`useState`는 정확히 두가지 변수를 가진 배열을 반환한다:

1. 현재 상태 (첫 랜더링동안, 이것은 주어진 `initialState` 값을 가진다.)
2.  [`set` 함수](https://react.dev/reference/react/useState#setstate)(다른 변수로 상태를 변경하고 리랜더링을 트리거시킨다.)

#### Caveats
- useState는 하나의 Hook 이다. 그래서 useState는 **컴포넌트 최상단**이나 커스텀 Hook들에서만 호출할 수 있다. 반복문이나 조건문에 useState를 호출할 수 없다. 만약 이러한 방식이 필요하다면 새로운 컴포넌트를 추출하고 상태(state)를 새로 만든 컴포넌트에 옮긴다.

- 엄격 모드에서, React는 **initializer function을 두 번 호출**한다. 이것은 [우발적인 비순수함을 찾는데 도움을 준다](https://react.dev/reference/react/useState#my-initializer-or-updater-function-runs-twice). 이것은 개발 모드에서만 동작하며 배포에는 영향을 주지 않는다. 만약 당신의 initializer function이 순수하다면 (물론 그래야 하지만) 이것은 실제 동작에 영향을 끼치지 않아야 한다. 두 개의 호출들 중 하나로 부터 나온 결과는 무시된다.

### `set` functions, like `setSomething(nextState)`

`useState`에 의해 반환되는 `set` 함수는 다른 변수로 상태를 업데이트할 수 있게 해주고 re-render를 트리거 시킨다. 다음 상태를 직접 전달하거나 이전 상태로 부터 새로운 상태를 계산하는 함수를 전달할 수 있다.

```jsx
const [name, setName] = useState('Edward');

function handleClick() {
	// pass the next state directly
	setName('Taylor');
	// pass the function that calculates it from the previous state
	setAge(a => a + 1);
	// ...
}
```

#### Parmeters
- `nextState`: 원하는 상태를 가지는 변수. 아무 타입의 변수나 가질 수 있지만, 함수의 경우 특별한 동작을 가진다.

> [!info]
> `nextState`로 함수를 전달하는 경우. 함수는 `updater` 함수로 다뤄진다. updater 함수는 순수해야만 하며, 오직 하나의 인자인 pending state를 가진다. 그리고 그것은 다음 상태를 반환해야만 한다. React는 updater 함수를 큐에 넣고 컴포넌트를 re-render 시킨다. 다음의 render 동안, React는 이전 상태에 큐에 있는 모든 updaters를 적용하여 다음 상태를 계산한다. ([예시](https://react.dev/reference/react/useState#updating-state-based-on-the-previous-state))

#### Returns
`set` 함수는 return 값을 가지지 않는다.

#### Caveats
- `set` 함수는 오직 **다음 render에 상태 변수를 업데이트**한다. 만약 `set` 함수를 호출한 후에 상태 변수를 읽으면, 호출하기 전에 화면에 있던 [이전 변수를 얻게될 것](https://react.dev/reference/react/useState#ive-updated-the-state-but-logging-gives-me-the-old-value)이다.

- 만약 제공하는 새로운 변수가 `Object.is`에 의해 비교되어 현재 `state`와 동일하다면, React는 **컴포넌트와 그 자식들의 re-rendering을 스킵**한다. 이것은 하나의 최적화이다. 비록 일부 경우에 React는 자식들의 render를 스킵하기 전에 여전히 컴포넌트를 호출할 필요가 있을 수 있지만, 그것은 코드에 영향을 끼치지 않는다.

- React는 [상태 업데이트를 일괄 처리](https://react.dev/learn/queueing-a-series-of-state-updates)한다. 일괄 처리는 **모든 event handlers를 실행하고** 그들의 `set` 함수들을 호출한 이후에 화면을 업데이트한다. 이것은 하나의 이벤트에 대한 다수의 re-render를 방지한다. React가 화면을 초기에 업데이트 하도록 해야하는 드믄 경우에는, 예를 들어 DOM에 접근해야하는 경우, [`flushSync`](https://react.dev/reference/react-dom/flushSync)를 사용할 수 있다.

## Summary
- `useState`는 컴포넌트에 **상태를 추가**할 수 있는 React Hooks이다.

- `initialState`에 함수를 전달하는 경우, 이 함수는 `initializer` 함수로 다뤄진다. `initializer` 함수는 컴포넌트를 초기화 할 때 **한 번 호출**되며 이후에는 이 함수를 **호출하지 않고 반환된 값을 사용**한다. 

- `set` 함수에 의한 상태 변경은 **다음 render에 반영**된다. 이것은 React에서 상태가 마치 **snapshot**처럼 동작하기 때문에 그러하다.

- state의 변화는 자바스크립트 Object의 메소드인 `Object.is`에 의해 평가되어 진다.

- React에서 상태 변화는 **일괄 처리(batches)** 된다. event handlers가 실행되었다면 각각 set function을 실행하고 re-render를 발생시키는 것이 아닌 모든 set function을 실행 이후 한 번의 re-render가 발생한다.

- [react.dev - useState](https://react.dev/reference/react/useState)
- [React Hooks에 취한다 - useState 15분만에 마스터하기 | 리액트 훅스 시리즈](https://www.youtube.com/watch?v=G3qglTF-fFI)