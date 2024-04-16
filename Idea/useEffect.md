`useEffect`는 [외부 시스템에 컴포넌트를 동기화](https://react.dev/learn/synchronizing-with-effects) 할 수 있게 해주는 React Hook 이다.

```jsx
useEffect(setup, dependencies?)
```

## Reference

### `useEffect(setup, dependencies?)`

Effect를 선언하기 위해 컴포넌트 최상단에 `useEffect`를 호출한다:

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function chatRoom({ roomId }) {
	const [serverUrl, setServerUrl] = useState('http://localhost:1234');

	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	}, [serverUrl, roomId])
}
```

[더 많은 예제는 아래를 확인하자](https://react.dev/reference/react/useEffect#usage)

### Parameters
- `setup`: Effect의 logic을 가진 함수. setup function은 또한 선택적으로 cleanup function을 반환할 수 있다. 컴포넌트가 DOM에 추가되었을 때, React는 setup function을 실행한다. 종속성(dependencies) 변화에 따른 모든 re-render 후에, React는 먼저 (만약 cleanup 함수를 제공했다면) 오래된 변수를 가지고 cleanup function을 실행하고 그 다음에 새로운 변수를 가지고 setup function을 실행한다. 이후에 컴포넌트가 DOM에서 제거되면, React는 cleanup function을 실행한다.

- optional `dependencies`: `setup` 코드의 내부에 참조된 모든 반응형 변수의 리스트. React는 `Object.is` 비교를 사용하여 이전 값을 가지고 각각의 종속성을 비교한다. 만약 이 인자를 빼먹는다면, Effect는 모든 컴포넌트의 re-render 이후에 재실행 될 것이다.
### Returns
`useEffect`는 `undefined`를 반환한다.

### Caveats
- `useEffect`는 Hook이다, 그렇기에 이것은 **컴포넌트의 최상단** 혹은 고유의 Hooks에서만 호출할 수 있다.

- 만약 **몇몇의 외부 시스템에 동기화할 것이 아니라면**, [아마도 Effect가 필요하지 않을 것이다.](https://react.dev/learn/you-might-not-need-an-effect)

- 엄격 모드를 사용할 때, React는 실제 첫 설정에 앞서 **개발 모드에서 하나의 임의의 setup+cleanup cycle**을 실행한다. 이것은 cleanup logic이 setup login과 그 setup이 무엇이던간에 그것을 멈추거나 그만두는 것을 cleanup logic이 "반영한다" 라는 것을 보장하는 stress-test이다. 만약 이것이 문제를 야기한다면, [cleanup function을 실행해야한다.](https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)

- 만약 종속성(dependencies)의 일부가 컴포넌트 안에 정의된 object 또는 function인 경우, 그들은 **필요보다 더 많은 Effect를 실행시킬** 위험이 있다. 이것을 해결하기 위해서는 불필요한 [object](https://react.dev/reference/react/useEffect#removing-unnecessary-object-dependencies) 그리고 [function](https://react.dev/reference/react/useEffect#removing-unnecessary-function-dependencies) 종속성을 제거해야 한다. 또한 Effect 외부로 [비반응형 로직](https://react.dev/reference/react/useEffect#reading-the-latest-props-and-state-from-an-effect)과 [상태 업데이트를 추출](https://react.dev/reference/react/useEffect#updating-state-based-on-previous-state-from-an-effect)할 수 있다.

- 만약 Effect가 클릭과 같은 상호작용으로 부터 발생하지 않았다면, React는 일반적으로 **Effect를 실행하기 전에 먼저 업데이트된 화면을 브라우저에 그린다.** 만약 Effect가 시각적인 (예를들어, tooltip의 위치 조정) 무언가를 하거나 주목할 만한 지연 (예를들어, flickers)이 있는 경우 `useEffect`를 [useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)로 교체하자.

- 비록 Effect가 클릭과 같은 상호작용에 의해 발생하더라도, **브라우저는 Effect 내부에 있는 상태 업데이트를 처리하기 전에 화면을 다시 그릴지도 모른다.** 

- Effect는 **오직 client에서만 동작한다.** server rendering 동안에는 실행되지 않는다.
## Usage

### Connecting to an external system
일부 컴포넌트는 페이지에 보여지는 동안 네트워크, 일부 브라우저 API, 서드 파티 라이브러리에 연결을 유지할 필요가 있다. 이러한 시스템은 React에 의해 제어되지 않기에, 그들은 *external* 이라 불린다.

[일부 외부 시스템에 컴포넌트를 연결 하기](https://react.dev/learn/synchronizing-with-effects) 위해 컴포넌트의 최상단에 useEffect를 호출한다.

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function chatRoom({ roomId }) {
	const [serverUrl, setServerUrl] = useState('http://localhost:1234');

	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	}, [serverUrl, roomId])
}
```

**React는 필요할 때마다 setup, cleanup 함수를 호출하며 이것은 여러 번 실행될 수 있다:**

1. 컴포넌트가 페이지에 추가되었을 때 `setup code`를 실행된다. (*mounts*).
2. `dependencies`가 변경된 모든 컴포넌트가 re-render된 이후에:
	- 먼저, 오래된 props, state를 가지고 `cleanup code`를 실행한다.
	- 이후에, 새로운 props, state를 가지고 `setup code`를 실행한다.
3. 컴포넌트가 페이지에서 제거된 이후 마지막으로 `cleanup code`를 실행한다 (*unmounts*).

**[버그를 찾는것에 도움을 주기](https://react.dev/learn/synchronizing-with-effects#step-3-add-cleanup-if-needed) 위해서, 개발 모드에서 React는 `setup` 이전에 임의의 `setup` 그리고 `cleanup` 함수를 실행한다.** 이러한 stress-test는 Effect 로직이 정확히 동작하는 것을 보장한다. 만약 이것이 시각적인 문제를 야기한다면, cleanup 함수가 일부 로직을 놓치고 있다는 것이다.

핵심은 유저가 한 번 (배포 단계에서) 호출되는 setup과 (개발 단계에서) setup -> cleanup -> setup sequence를 구별할 수 없어야 한다는 것이다. [일반적인 해답을 살펴보자.](https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)

[모든 Effect를 독립된 프로세스로 작성](https://react.dev/learn/lifecycle-of-reactive-effects#each-effect-represents-a-separate-synchronization-process)하고 [한 번에 하나의 setup/cleanup cycle을 생각](https://react.dev/learn/lifecycle-of-reactive-effects#thinking-from-the-effects-perspective)할 수 있도록 해야한다.

> [!note]
> Effect는 (chat service 같은) 외부 시스템에 [컴포넌트를 동기화할 수 있게](https://react.dev/learn/synchronizing-with-effects) 해준다. 여기서 외부 시스템이란 React에 의해 제어되지 않는 코드, 예를들어 timer, event subscription, third-party animation libray 등을 의미한다. 
> 
> **만약 외부 시스템에 연결이 필요하지 않은 경우, 아마도 Effect는 필요하지 않다.**

### Wrapping Effects in custom Hooks
만약 자주 Effect를 작성해야 한다는 것을 발견하면, 그것은 보통 컴포넌트가 의존하는 공통 동작을 [custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)로 추출할 필요가 있다는 신호이다.

### Controlling a non-React widget
때때로, 컴포넌트의 prop 혹은 state에 외부 시스템을 동기화 하고 싶을 수 있다. 

### Fetching data with Effects
컴포넌트에 데이터를 가져오기 위해 Effect를 사용할 수 있다.

예제에서 `false`로 초기화 되었다가 cleanup 동안 `true`로 설정되는 `ignore` 변수에 주목하자. 이것은 ["race conditions" 을 겪지 않도록](https://maxrozen.com/race-conditions-fetching-data-react-with-useeffect) 보장해준다: 네트워크 응답은 요청을 보낸 것과 다른 순서로 도착할 수 있다. 

Effect에 직접 데이터 패칭을 작성하는 것은 반복적이고 캐싱과 서버 랜더링같은 최적화를 추가하는데 이후에 어려움을 겪게 된다.

> [!note] What are good alternatives to data fetching in Effects?
> Effect 내부에 `fetch`를 작성하는 것은 특히 완전한 client-side 어플리케이션에서 [data fetch를 위한 인기있는 방법](https://www.robinwieruch.de/react-hooks-fetch-data/)이다. 하지만 이것은 매우 수동적인 접근이며 상당한 단점을 가진다:
>  
>  - **Effect는 서버에서 동작하지 않는다.**
>  - **Effect에서 직접 Fetching 하는 것은 "network waterfalls"를 만들기 쉽다.**
>  - **Effect에서 직접 Fetching 하는 것은 보통 데이터를 preload 하거나 cache 하지 않는다는 것을 의미한다.**
>  - **이러한 동작은 매우 편의적이지 않다.**

### Specifying reactive dependencies
**Effect의 종속성(dependencies)를 "선택" 할 수 없음에 주목하자.** Effect의 코드에 사용되는 모든 `reactive value`는 종속성으로 선언되어야만 한다.

**[Reactive values](https://react.dev/learn/lifecycle-of-reactive-effects#effects-react-to-reactive-values)는 컴포넌트 내부에 직접적으로 정의된 props 그리고 모든 변수와 함수를 포함한다.**

**종속성을 제거하기 위해서는 [linter에 그것이 종속성에 불필요하다는 것을 입증](https://react.dev/learn/removing-effect-dependencies#removing-unnecessary-dependencies)해야만한다.**

**만약 Effect가 어떠한 reactive value도 사용하지 않고 있다면 종속성 배열은 빈 배열이어야만 한다.** [빈 종속성을 가진 Effect](https://react.dev/learn/lifecycle-of-reactive-effects#what-an-effect-with-empty-dependencies-means)는 컴포넌트에 어떠한 props 혹은 state의 변화가 있더라도 재실행되지 않는다.

> [!caution] Pitfall
> 만약 코드베이스에서 아래와 같이 linter를 억제하는 Effect를 가지고 있는 경우:
> ```jsx
> useEffect(() => {  
> 	// ...  
> 	// 🔴 Avoid suppressing the linter like this:  
> 	// eslint-ignore-next-line react-hooks/exhaustive-deps  
> }, []);
> ```
> **종속성이 코드와 일치하지 않는 경우, 버그를 발생시킬 큰 위험이 있다.**

#### Examples of passing reactive dependencies

##### 1. Passing a dependency array
만약 종속성(dependencies)를 명시하는 경우, Effect는 **변경된 종속성을 가지고 initial render 그리고 re-render 후에 실행된다.**

##### 2. Passing an empty dependency array
만약 Effect가 어떠한 reactive values도 사용하지 않는 경우, 오직 **initial render 이후에만 실행된다.**

##### 3. Passing no dependency array at all
만약 종속성 배열을 전혀 전달하지 않는 경우, Effect는 컴포넌트의 **매 render와 re-render 후에 실행된다.**

### Updating state based on previous state from an Effect

```jsx
function Counter() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		const intervalId = setInterval(() => {
			// You want to increment the counter every second ...
			setCount(count + 1); 
		}, 1000)
		return () => clearInterval(intervalId);
	// 🚩 ... but specifying `count` as a dependency always resets the interval.
	}, [count]); 
	
}
```

To fix: 

```jsx
function Counter() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		const intervalId = setInterval(() => {
			 // ✅ Pass a state updater
			setCount(c => c + 1); 
		}, 1000)
		return () => clearInterval(intervalId);
	// ✅ Now count is not a dependency
	}, []); 
	
}
```
### Removing unnecessary object dependencies

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'http://localhost:1234';

function chatRoom({ roomId }) {
	const [message, setMessage] = useState('');

	// 🚩 This object is created from scratch on every re-render
	const options = {
		serverUrl: serverUrl,
		roomId: roomId
	}

	useEffect(() => {
		// It's used inside the Effect
		const connection = createConnection(options);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	// 🚩 As a result, these dependencies are always different on a re-render
	}, [options])
}
```

To fix:

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'http://localhost:1234';

function chatRoom({ roomId }) {
	const [message, setMessage] = useState('');

	useEffect(() => {
		// ✅ create the object inside the Effect
		const options = {
		serverUrl: serverUrl,
		roomId: roomId
		};
		const connection = createConnection(options);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	}, [roomId])
}
```

### Removing unnecessary function dependencies

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'http://localhost:1234';

function chatRoom({ roomId }) {
	const [message, setMessage] = useState('');
	// 🚩 This function is created from scratch on every re-render
	function createOptions() {
		return {
			serverUrl: serverUrl,
			roomId: roomId	
		}
	}

	useEffect(() => {
		// It's used inside the Effect
		const options = createOptions();
		const connection = createConnection(options);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	// 🚩 As a result, these dependencies are always different on a re-render
	}, [createOptions])
}
```

To fix:

```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'http://localhost:1234';

function chatRoom({ roomId }) {
	const [message, setMessage] = useState('');

	useEffect(() => {
		// ✅ create the function inside the Effect
		function createOptions() {
			return {
				serverUrl: serverUrl,
				roomId: roomId	
			}
		}
		
		const options = createOptions();
		const connection = createConnection(options);
		connection.connect();
		return () => {
			connection.disconnect();
		};
	
	}, [roomId])
}
```

### Reading the latest props and state from an Effect
**그러나, 때때로 종속성 배열에 "reacting" 없이 Effect로 부터 최신의 props와 state를 읽고 싶을 수 있
다.**

```jsx
function Page({ url, shoppingCart }) {
	useEffect(() => {
		logVisit(url, shoppingCart.length);
	}, [url, shoppingCart]); // ✅ All dependencies declared
	// ...
}
```

**만약 `url`이 변경된 이후에 새로운 페이지 방문을 기록하고 싶지만 `shoppingCart`의 변화에는 그러고 싶지 않다면?**

그러나 비록 Effect 내부로부터 호출 될지라도 일부 코드가 변화에 "react" 하기를 원치 않다고 표현할 수 있다. 

[`useEffectEvent`](https://react.dev/reference/react/experimental_useEffectEvent) Hook을 이용하여 [Effect Event를 선언](https://react.dev/learn/separating-events-from-effects#declaring-an-effect-event)한다:

```jsx
function Page({ url, shoppingCart }) {
	const onVisit = useEffectEvent(visitedUrl => {
		logVisit(visitedUrl, shoppingCart.length);
	})

	useEffect(() => {
		onVisit(url);
	}, [url]); // ✅ All dependencies declared
	// ...
}
```

**Effect Events는 reactive 하지 않고 항상 Effect의 종속성에서 빠져야만 한다.**

[어떻게 Effect Events가 reactive 와 non-reactive code를 나누는지 좀 더 읽어보자.](https://react.dev/learn/separating-events-from-effects#reading-latest-props-and-state-with-effect-events)
### Displaying different content on the server and the client

```jsx
function MyComponent() {
	const [didMount, setDidMount] = useState(false);

	useEffect(() => {
		setDidMount(true);
	}, []);

	if (didMount) {
		// ... return client-only JSX ...
	} else {
		// ... return initial JSX ...
	}
}
```

## Troubleshooting

### My Effect runs twice when the component mounts
엄격 모드가 설정되고 개발 환경에서 React는 실제 setup 전에 한 번 여분의 setup / cleanup 함수를 실행한다.

### My Effect runs after every re-render
먼저, 종속성 배열에 명시하는 것을 잊지 않았는지 검사한다:

```jsx
useEffect(() => {
	// ...
}) // 🚩 No dependency array: re-runs after every render!
```

만약 의존성 배열에 명시를 했음에도 Effect가 여전히 반복하여 재실행되고 있다면, 그것은 종속성 중 하나가 매 re-render 마다 다르기 때문이다.

콘솔에 수동으로 종속성을 기록해봄으로써 이 문제를 디버깅 할 수 있다:

```jsx
useEffect(() => {
	// ..
}, [serverUrl, roomId]);

console.log([serverUrl, roomId]);
```
### My Effect Keeps re-running in an infinite cycle

### My cleanup login runs even though my component didn't unmount

### My Effect does something visual, and I see a flicker before it runs

## Summary
- `useEffect`는 **외부의 시스템에 컴포넌트를 동기화 하기 위해서** 사용하는 Hook이다. 여기서 외부 시스템이란 React에 의해 제어되지 않는 코드, 예를들어 timer, event subscription, third-party animation libray 등을 의미한다. **만약 외부 시스템에 연결이 필요하지 않은 경우, 아마도 Effect는 필요하지 않다.**

- 컴포넌트가 *Mount* 되면 Effect의 `setup code`가 실행된다. 이후 *Update*가 발생하였을 때, `cleanup code`를 실행하고 다시 `setup code`를 실행하며 컴포넌트가 *UnMount* 되면 마지막으로 `cleanup code`를 실행한다.

- cleanup 함수를 사용하면 `useEffect`를 통해 Data fetching을 할 때 발생하는 **"race condition"** 을 해결할 수 있다. 
  
  `boolean flag`를 사용하여 cleanup 시 이전 결과를 사용하지 않도록 하여 "race condition" 문제를 해결할 수 있다. (여전히 다수의 요청이 진행 중이라는 의미에서 race-condition이 남아 있지만, 오직 마지막으로부터 나온 결과만을 사용한다) 
  
  Web API인 `AbortController`를 사용하면 또한 이 문제를 해결할 수 있다. AbortController는 Web 요청을 중단할 수 있는 객체를 제공한다. 따라서 이를 cleanup 단계에 사용함으로써 이전의 (오래된) 요청을 중단하고 최신의 요청의 결과만을 반영할 수 있게된다. (인터넷 익스플로러 에서 사용하지 못하지만 대부분 이것을 사용하지 않기 때문에 유저의 **대역폭(bandwidth)** 을 생각하면 AbortController를 사용하는 것이 더 좋다.)

- `useEffectEvent` Hook을 사용하면 종속성에 명시하지 않고 state나 props 같은 reacting value를 사용할 수 있다.


## Memo
- 비교를 위해 `Object.is`를 사용하는 이유? 