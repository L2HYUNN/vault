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

- 랜더링 중에 `set` 함수를 호출하는 것은 오직 현재 랜더링중인 컴포넌트 내에서만 가능하다. React는 그것의 결과를 버리고 즉시 새로운 상태를 가지고 render를 시도한다. 이 패턴은 드물게 필요하지만 이것을 **이전 render로 부터 정보를 저장**하기 위해 사용할 수 있다. ([예시](https://react.dev/reference/react/useState#storing-information-from-previous-renders))

- 엄격 모드에서, React는 updater 함수를 두 번 호출한다. 그것은 [우발적인 비순수함을 찾는데](https://react.dev/reference/react/useState#my-initializer-or-updater-function-runs-twice) 도움을 준다. 이것은 개발 모드에서만 동작하며 배포 이후에 영향을 미치지 않는다. 만약 updater 함수가 순수하다면(그래야 겠지만), 이것은 동작에 영향을 끼치지 않아야 한다. 두 번의 호출들 중 하나로 부터의 결과는 무시된다.

## Usage

### Adding state to a component
하나 혹은 더 많은 [상태 변수](https://react.dev/learn/state-a-components-memory)들을 선언하기 위해서 컴포넌트의 최상단에 `useState`를 호출한다.

```jsx
import { useState } from 'react';

function MyComponent() {
	const [age, setAge] = useState(42);
	const [name, setName] = useState('Taylor');
	//...
}
```

[array destructuring](https://javascript.info/destructuring-assignment)을 이용하여 [something, setSomething] 같이 상태 변수들을 이름붙인다.

`useState`는 정확히 두 가지 아이템을 가진 배열을 반환한다:

1. 상태 변수의 `현재 상태(current state)`, 초기에 제공된 `초기 상태(initial state)`로 설정된다.
2. `set 함수(set function)`, 상호작용에 대한 응답에 대한 어떤 다른 변수로 변경시키는 함수

화면에 있는 무언가를 업데이트 시키기 위해서는, 일부 새로운 상태를 가지고 `set` 함수를 호출해야한다:

```jsx
function handleClick() {
	setName('Robin');
}
```

React는 다음 상태를 저장하고 새로운 변수를 가지고 컴포넌트를 다시 랜더하며, UI를 업데이트한다.

> [!caution] Pitfall
> `set` 함수를 호출하는 것은 [**이미 실행된 코드에 있는 현재 상태를 변경시키지 않는다**](https://react.dev/reference/react/useState#ive-updated-the-state-but-logging-gives-me-the-old-value):
> 
> ```jsx
> function handleClick() {
> 	setName('Robin');
> 	console.log(name); // Still "Taylor"!
> }
> ```
> 그것은 오직 다음 랜더 부터 `useState` 가 반환하는 것에만 영향을 미친다. 

### Updating state based on the previous state
`age`는 `42`라고 가정하자. 아래의 handler 함수는 `setAge(age + 1)`을 3번 호출한다:

```jsx
function handleClick() {
	setAge(age + 1); // setAge(42 + 1)
	setAge(age + 1); // setAge(42 + 1)
	setAge(age + 1); // setAge(42 + 1)
}
```

그러나 한 번의 클릭 후에, `age`는 45가 아닌 43을 가진다. 이것은 `set` 함수를 호출하는 것이 이미 실행된 코드에 있는 `age` 상태 변수를 [업데이트 하지 않기 때문](https://react.dev/learn/state-as-a-snapshot)이다. 따라서 각각의 `setAge(age + 1)` 호출은 `setAge(43)`이 된다.

이러한 문제를 해결하기 위해, 다음 상태를 전달하는 것 대신에 `setAge`에 **updater 함수를 전달**한다:

```jsx
function handleClick() {
	setAge(a => a + 1); // setAge(42 => 43)
	setAge(a => a + 1); // setAge(43 => 44)
	setAge(a => a + 1); // setAge(44 => 45)
}
```

여기에, `a => a + 1`은 updater 함수이다. 이것은 `pending state`를 가지고 그것으로 부터 `next state`를 계산한다.

React는 updater 함수를 [queue](https://react.dev/learn/queueing-a-series-of-state-updates)에 넣는다. 이후 다음 랜더 동안, 동일한 순서로 그들을 호출한다:

1. `a => a + 1` 은 pending state로 `42`를 받고 next state로 `43`을 반환한다.
2. `a => a + 1` 은 pending state로 `43`를 받고 next state로 `44`을 반환한다.
3. `a => a + 1` 은 pending state로 `44`를 받고 next state로 `45`을 반환한다.

더 이상 큐에 있는 updater가 없기 때문에 React는 마지막에 현재 상태로 `45`를 저장한다.

관습적으로, pending state의 인자로 `age`의 `a` 와 같은 상태 변수의 첫 번째 문자를 사용한다. 그러나 `prevAge` 혹은 더욱 분명하게 표현할 수 있는 다른 이름을 사용할 수 있다.

React는 [순수함](https://react.dev/learn/keeping-components-pure)을 증명하기 위해 개발 모드에서 [updater 함수를 두 번 호출](https://react.dev/reference/react/useState#my-initializer-or-updater-function-runs-twice)한다.

> [!question] Is using an updater always preferred?
> 이전 상태로 부터 계산된 설정의 상태라면 `setAge(a => a + 1)` 과 같은 코드를 항상 작성하도록 권장받았을 수 있다. 그것에 문제는 없지만, 그렇다고 항상 필요한 것은 아니다.
> 
> 대부분의 경우에, 두 접근법에 차이는 없다. React는 항상 클릭과 같은 의도적인 유저 액션이 `age` 상태 변수가 다음 클릭전에 업데이트 되는 것을 보장한다. 이것은 click handler 함수가 초기에 "오래된"  `age` 변수를 볼 위험이 없다는 것을 의미한다.
> 
> 그러나, 동일한 이벤트에 다수의 업데이트를 발생시키는 경우, undaters는 도움이 될 수 있다. 그들은 또한 상태 변수 자체에 접근하는 것이 불편한 경우(re-render를 최적화 할 때 마주칠지도 모른다) 도움이 될 수 있다.
> 
> 만약 간결한 문법 보다 좀 더 일관성 있는 문법을 선호하는 경우, 이전 상태로 부터 계산된 상태의 경우 항상 updater 함수를 사용하는 것이 합리적일 수 있다. 만약 일부 다른 상태 변수로 구성된 이전 상태로 부터 계산하는 경우, 하나의 객체로 그들을 조합하고 [reducer를 사용하기](https://react.dev/learn/extracting-state-logic-into-a-reducer)를 원할 수도 있다.

### Updating objects and arrays in state
객체나 배열을 상태에 넣어 사용할 수 있다. React에서 상태는 read-only로 다뤄진다. 그렇기에 존재하는 객체를 변경하기 보다 그것을 새로운 것으로 **교체해야만한다**. 예


## Summary
- `useState`는 컴포넌트에 **상태를 추가**할 수 있는 React Hooks이다.

- `initialState`에 함수를 전달하는 경우, 이 함수는 `initializer` 함수로 다뤄진다. `initializer` 함수는 컴포넌트를 초기화 할 때 **한 번 호출**되며 이후에는 이 함수를 **호출하지 않고 반환된 값을 사용**한다. 

- `set` 함수에 의한 상태 변경은 **다음 render에 반영**된다. 이것은 React에서 상태가 마치 **snapshot**처럼 동작하기 때문에 그러하다.

- state의 변화는 자바스크립트 Object의 메소드인 `Object.is`에 의해 평가되어 진다.

- React에서 상태 변화는 **일괄 처리(batches)** 된다. event handlers가 실행되었다면 각각 set function을 실행하고 re-render를 발생시키는 것이 아닌 모든 set function을 실행 이후 한 번의 re-render가 발생한다.

- `flushSync`를 사용하면 re-render 전에 **즉시 DOM을 업데이트**시킬 수 있다.

- [react.dev - useState](https://react.dev/reference/react/useState)
- [React Hooks에 취한다 - useState 15분만에 마스터하기 | 리액트 훅스 시리즈](https://www.youtube.com/watch?v=G3qglTF-fFI)