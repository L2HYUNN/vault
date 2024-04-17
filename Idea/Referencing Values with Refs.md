컴포넌트가 정보를 "기억하기" 원할 때, 하지만 그 정보가 새로운 [render를 트리거](https://react.dev/learn/render-and-commit) 시키지 않기를 원하는 경우, ref를 사용할 수 있다.

## Adding a ref to your component

```js
import { useRef } from 'react'
```

```jsx
const ref = useRef(0);
```

```js
{
	current: 0 // The value you passed to useRef
}
```

`ref.current` 속성을 통해 현재 값에 접근할 수 있다. 이 변수는 의도적으로 변경이 가능하다. 이것은 읽고 쓰기가 모두 가능하다는 것을 의미한다.

현재 ref는 number를 가리키지만 [state](https://react.dev/learn/state-a-components-memory) 처럼 어떤것도 가리킬 수 있다: string, object 심지어 함수도 가능하다. state와 달리 ref는 읽고 수정할 수 있는 `current` 속성을 가진 평범한 자바스크립트 객체이다.

**매 증가마다 컴포넌트가 re-render 되지 않는것**에 주목하자. state 처럼 refs는 re-render 사이에 React에 의해 유지된다. 하지만 state를 설정하는 것은 컴포넌트를 re-render 시킨다. ref의 변경은 re-render를 유발하지 않는다.

## Example: building a stopwatch
정보의 일부가 랜더링에 사용되는 경우, 상태를 유지해야 한다. 정보의 일부가 오직 이벤트 핸들러에 필요하고 리랜더링을 필요로 하지 않는 변경이라면 ref를 사용하는 것이 좀 더 효율적이다.

## Differences between refs and state
랜더링 도중에 `ref.current`를 읽는 것은 신뢰할 수 없는 코드를 만들 수 있다.

> [!note] How does useRef work inside?
> `useState`와 `useRef`가 React에 의해 제공되지만, 원칙적으로 `useRef`는 `useState` 위에 구현될 수 있다. React 내부를 생각해보면, `useRef`가 아래와 같이 구현된다:
> 
> ```jsx
> // Inside of React
> function useRef(initialValue) {
> 	const [ref, unused] = useState({ current: initialValue });
> 	return ref;
> }
> ```
> 
> 첫 랜더링에, `useRef`는 `{ current: initialValue }`를 반환한다. 이 객체는 React에 의해 저장되기 때문에, 다음 랜더링에 동일한 객체를 반환할 것이다. 예제에서 state setter를 사용하지 않는 것에 주목하자. `useRef`는 항상 같은 객체를 반환하기 때문에 이것은 불필요하다.
> 
> React는 실제로 충분히 흔하기 때문에 빌트인 버전의 `useRef`를 제공한다. 하지만 `useRef`를 setter가 없는 일반적인 상태 변수로 생각할 수 있다. 만약 OOP에 친숙하다면, refs는 인스턴스 필드를 떠올리게 할 지 모른다. - 하지만 `this.something` 대신에 `somethingRef.current` 를 사용한다.

## When to use refs
전형적으로 컴포넌트가 React "step outside"할 필요가 있고 외부 API (종종 브라우저 API는 컴포넌트의 외형에 영향을 끼치지 않는다)와 통신할 필요가 있을 때 ref를 사용할 것이다. 여기 이러한 드문 상황이 몇가지 있다:

- [timeout IDs](https://developer.mozilla.org/docs/Web/API/setTimeout)를 저장할 때
- [DOM 요소](https://developer.mozilla.org/docs/Web/API/Element)를 저장하고 조종할 때
- JSX를 연산할 필요가 없는 다른 객체를 저장할 때

## Summary

- 리랜더링을 발생시키지 않으면서, 랜더링 사이에 상태를 유지하고 싶은 경우 `useRef`를 사용할 수 있다.

- 랜더링 중에 `current` 변수를 읽거나 쓰지 말아야 한다.