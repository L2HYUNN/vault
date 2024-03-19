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
> 이것은 `initializer` 함수로 다뤄진다. 이 함수는 순수 해야하며, 인자를 가져서는 안되고 타입을 가진 변수를 반환해야한다. React는 `initializer` 함수를 컴포넌트를 초기화할 때 호출하고, 초기 상태에 반환되는 변수를 저장한다. 

#### Returns
`useState`는 정확히 두가지 변수를 가진 배열을 반환한다:

1. 현재 상태 (첫 랜더링동안, 이것은 주어진 `initialState` 값을 가진다.)
2.  [`set` 함수](https://react.dev/reference/react/useState#setstate)(다른 변수로 상태를 변경하고 리랜더링을 트리거시킨다.)

#### Caveats
- useState는 하나의 Hook 이다. 



- [react.dev - useState](https://react.dev/reference/react/useState)
- [React Hooks에 취한다 - useState 15분만에 마스터하기 | 리액트 훅스 시리즈](https://www.youtube.com/watch?v=G3qglTF-fFI)