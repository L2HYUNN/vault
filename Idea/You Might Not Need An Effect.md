Effect는 React 패러다임으로부터의 탈출구이다. 그들은 React의 "밖으로 나가게" 해주며 당신의 컴포넌트를 일부 외부 시스템(non-React 위젯, 네트워크 또는 브라우저 DOM)에 동기화 할 수 있게 해준다. 만약 관련된 외부 시스템이 없다면(예를들어, 만약 당신이 일부 props 혹은 state가 바뀔때 컴포넌트의 상태가 업데이트 되기를 원한다면). 당신은 Effect를 사용할 필요가 없다. 불필요한 Effect를 제거하는 것은 당신의 코드를 더 쉽게 이해하고 더 빠르게 실행하고 덜 오류가 발생하게 만들어줄 것이다.

> [!info] 당신이 배울 것
> - Why and how to remove unnecessary Effects from your components
> - How to cache expensive computations without Effects
> - How to reset and adjust component state without Effects
> - How to share logic between event handlers
> - Which logic should be moved to event handlers
> - How to notify parent components about changes

## How to remove unnecessary Effects
Effect가 필요하지 않은 흔한 두 가지 경우가 존재한다:

- **랜더링 동안 데이터를 변경하는데에는 Effect가 필요하지 않다.** 예를들어 화면에 보이기 전에 리스트를 필터링하고 싶다고 해보자. 당신은 리스트가 변경되었을 때 상태 변수를 업데이트 하는 Effect를 작성하고 싶을 수도 있다. 하지만 이것은 비효율적인 일이다. 상태를 업데이트할 때, React는 처음으로 당신의 컴포넌트 함수를 호출하고 무엇이 화면에 보일지 계산한다. 다음에 React는 DOM에 이러한 변경 사항을 ["commit"](https://react.dev/learn/render-and-commit)할 것이고 화면을 업데이트할 것이다. 다음에 React는 Effect를 실행할 것이다. 만약 당신의 Effect가 또한 즉시 상태를 업데이트 한다면, 이것은 위에서 이야기한 과정을 전부 다시 실행할 것이다. 불필요한 render가 전달되는 것을 피하기 위해서, 컴포넌트의 최상단에 모든 데이터를 변경하라. 그 코드는 자동으로 당신의 props나 상태가 변경되었을 때 재실행될 것이다.
- **유저 이벤트를 다루는데 Effect가 필요하지 않다.** 예를들어 `/api/buy` POST 요청을 보내고 유저가 상품을 구매할 때 알림을 보여주고 싶다고 해보자. Buy 버튼 클릭 이벤트 핸들러에서 당신은 정확히 무엇이 일어났는지 알고 있다. Effect가 실행되는 시간에 당신은 유저가 했던 행동(예를들어, 어떤 버튼이 클리되었는지)을 알지 못한다. 이것이 당신이 보통 유저 이벤트를 응답하는 이벤트 핸들러에서 다루는 이유이다. 

정확한 직관을 얻는데 도움을 얻기 위해 흔하고 구체적인 몇 가지 예시를 살펴보자.

## Updating state based on props or state
두 가지 상태 변수(`firstName` 그리고 `lastName`)를 가진 컴포넌트를 가지고 있다고 가정해보자. 당신은 그들을 연결함으로써 그들로부터 `fullName`을 계산하고 싶다. 더욱이, `firstName` 혹은 `lastName`이 변경될 때마다 당신은 `fullName`을 업데이트하기를 원한다. 당신은 본능적으로 `fullName` 상태 변수를 추가하고 Effect에서 그것을 업데이트할지도 모른다:

```jsx
function Form() {
	const [firstName, setFirstName] = useState('Taylor');
	const [lastName, setLastName] = useState('Swift');

	// 🔴 Avoid: redundant state and unnecessary Effect
	const [fullName, setFullName] = useState('');
	useEffect(() => {
		setFullName(firstName + ' ' + lastName);
	}, [firstName, lastName]);
	//...
}
```

이것은 필요 이상으로 복잡하다. 또한 그것은 오래된 변수인 `fullName`을 가지고 전체적인 랜더 패스를 실핸 다음 즉시 업데이트된 새로운 변수를 가지고 리랜더링을 실행하기 때문에  비효율적이다. 상태 변수와 Effect를 제거하자:

```jsx
function Form() {
	const [firstName, setFirstName] = useState('Taylor');
	const [lastName, setLastName] = useState('Swift');
	
	// ✅ Good: calculated during rendering
	const fullName = firstName + ' ' + lastName;
	// ...
}
```

**기존에 존재하는 props 혹은 state로 부터 계산될 수 있는 것이 있는 경우, [그것을 상태에 넣지 말아라.](https://react.dev/learn/choosing-the-state-structure#avoid-redundant-state) 대신에 랜더링 동안 그것을 계산하라.** 이것은 당신의 코드를 더 빠르게(당신은 여분의 "계단식" 업데이트를 피한다) 더 쉽게(당신은 일부 코드를 제거한다), 그리고 덜 에러가 발생하게(서로의 싱크에서 벗어난 다른 상태 변수로 인해 야기되는 버그를 피한다)만들 수 있다. 만약 이 방법이 당신에게 새롭게 느껴진다면, [React로 생각하기](https://react.dev/learn/thinking-in-react#step-3-find-the-minimal-but-complete-representation-of-ui-state)는 상태에 무엇을 넣어야 하는지 설명해준다.

## Caching expensive calculations
아래의 컴포넌트는 props로 전달받은 `todos`를 이용하고 `filter` prop에 따라 그들을 필터링함으로써 `visibleTodos`를 계산한다. 당신은 상태에 그 결과를 저장하고 Effect로 부터 그것을 업데이트 하고 싶은 기분이 들 수 있다.: 

```jsx
function TodoList({ todos, filter }) {
	const [newTodo, setNewTodo] = useState('');

	// 🔴 Avoid: redundant state and unnecessary Effect  
	const [visibleTodos, setVisibleTodos] = useState([]);  
	useEffect(() => {  
		setVisibleTodos(getFilteredTodos(todos, filter));  
	}, [todos, filter]);  
	// ...
}
```

앞에 예시와 같이, 이것은 불필요하며 비효율적인 일이다. 첫번째로, 상태와 Effect를 제거하라:

```jsx
function TodoList({ todos, filter }) {
	const [newTodo, setNewTodo] = useState('');

	// ✅ This is fine if getFilteredTodos() is not slow.  
	const visibleTodos = getFilteredTodos(todos, filter);  
	// ...
}
```

일반적으로 이 코드는 괜찮다! 하지만 아마도 `getFilteredTodos()`는 많은 `todos`를 가지고 있다면 느릴 수 있다. 이 경우 `newTodo`와 같은 직접적으로 관계되지 않은 일부 상태 변수가 변경될 때`getFilteredTodos()` 가 재계산되는 것을 원치않을 수 있다.

이때, [`useMemo`](https://react.dev/reference/react/useMemo) Hook으로 그것을 감쌈으로써 비싼 연산을 캐쉬(혹은 ["메모이즈"](https://en.wikipedia.org/wiki/Memoization))할 수 있다.

```jsx
import { useMemo, useState } from 'react';

function TodoList({ todos, filter }) {
	const [newTodo, setNewTodo] = useState('');

	// ✅ Does not re-run unless todos or filter change
	const visibleTodos = useMemo(() => {
		return getFilteredTodos(todos, filter);
	}, [todos, filter]);
	// ...
}
```

혹은, 한 줄에 작성하면 다음과 같다: 

```jsx
function TodoList({ todos, filter }) {
	const [newTodo, setNewTodo] = useState('');
	
	// ✅ Does not re-run getFilteredTodos() unless todos or filter change
	const visibleTodos = useMemo(() => getFilteredTodos(todos, filter), [todos, filter]);
	// ...
}
```

이것은 React에게 `todos` 혹은 `filter`가 변경되지 않는 이상 내부 함수가 재실행되는 것을 원치 않는다고 말하는 것이다. React는 처음 render 동안 `getFilteredTodos()`의 반환 값을 기억할 것이다. 다음 랜더동안, 그것은 `todos` 혹은 `filter`가 변경되었는지 체크할 것이다. 만약 그들이 지난번과 같은 값을 유지한다면, `useMemo`는 저장되어 있던 지난 결과를 반환할 것이다. 하지만 만약 그들이 다르다면, React는 내부 함수를 재호출 할 것이다(그리고 그것의 결과를 저장한다).

> [!example] DEEP DIVE
> ### How to tell if a calculation is expensive?
> 
> 일반적으로, 수천의 객체를 생성하거나 반복하지 않는 이상, 아마도 비싼 것은 아닐 것이다. 만약 조금 더 확신을 얻고 싶다면, 코드의 일부에 사용되는 시간을 측정하기 위해 console log를 추가할 수 있다:
> 
> ```jsx
> console.time('filter array');
> const visibleTodos = getFilteredTodos(todos, filter);
> console.timeEnd('filter array');
> ```
>
> 측정하려는 동작을 수행하자 (예를들어, input에 타이핑하는것과 같은). 이후 콘솔에 `filter array: 0.15ms`와 같은 로그들을 볼 수 있다. 만약 전체 로그된 시간이  상당한 양 (`1ms` 혹은 그 이상)으로 누적된 경우, 연산을 메모이즈 하는 것이 맞을 것이다. 실험으로써, 당신은 `useMemo`를 이용하여 연산을 감싸고 동작에 대한 종합적인 로그 시간이 줄었는지 아닌지를 입증할 수 있다:
> 
>```jsx
> console.time('filter array');
> const visibleTodos = useMemo(() => {
> 	return getFilteredTodos(todos, filter); // Skipped if todos and filter haven't changed
> }, [todos, filter]);
> console.timeEnd('filter array');
>``` 
>
> `useMemo`는 첫 랜더를 더 빠르게 만들지 않는다. 그것은 오직 업데이트에 불필요한 작업을 스킵하는데 도움을 준다.
> 
> 유저가 사용하는 것 보다 당신의 머신이 아마도 더 빠를 것이라는 것을 명심해라. 그렇기에 인공적으로 느려진 환경에서 성능을 테스트하는 것은 좋은 생각이다. 예를들어, 크롬은 이것을 위해 [CPU Throttling](https://developer.chrome.com/blog/new-in-devtools-61/#throttling) 옵션을 제공한다.
> 
> 또한 개발 모드에서 성능을 측정하는 것은 대부분 정확한 결과를 주지 못한다는 것에 주목하자. (예를들어, 엄격모드가 활성화되어 있는 경우, 각각의 컴포넌트가 한 번이 아닌 두 번 랜더되는 것을 보게될 것이다.) 대부분 정확한 시간을 측정하기 위해서는 프로덕션 모드로 앱을 빌드하고 유저가 사용하는 환경에서 그것을 테스트하자.
> 

## Resetting all state when a prop changes
`ProfilePage` 컴포넌트는 `userId` prop을 전달받는다. 페이지는 comment input을 포함하고 그것의 변수를 유지하기 위해 `comment` 상태 변수를 사용한다. 어느날, 당신은 문제를 발견했다: 하나의 profile로 부터 또 다른 profile로 이동하려 할 때, `comment` 상태가 초기화 되지 않는다. 결과적으로 우연치않게 올바르지 않은 유저 프로필에 comment를 작성하기 쉬워지게 됐다. 이러한 문제를 해결하기 위해, 당신은 `userId`가 변경될 때마다 `comment` 상태 변수를 지우고 싶다.: 

```jsx
export default function ProfilePage({ userId }) {
	const [comment, setComment] = useState('');

	// 🔴 Avoid: Resetting state on prop change in an Effect  
	useEffect(() => {  
		setComment('');  
	}, [userId]);  
	// ...
}
```

이것은 비효율적이다 왜냐하면 `ProfilePage` 그리고 그것의 자식은 오래된 변수를 가지고 초기에 랜더링 될 것이고 이후 다시 랜더링을 수행할 것이기 때문이다. 그것은 또한 당신이 `ProfilePage` 내부에 일부 상태를 가지는 모든 컴포넌트에 이것을 수행할 필요가 있기 때문에 복잡하다. 예를들어, 만약 comment UI가 중첩되었다면, 중첩된 comment 상태 또한 지워내기를 원할 것이다.

대신에, React에게 각각의 유저 프로필이 개념적으로 다른 프로필이라는 것을 분명한 키를 제공함으로써 말해줄 수 있다. 컴포넌트를 두가지로 분리하고 밖의 컴포넌트로부터 안의 컴포넌트에 `key` 속성을 전달하자.: 

```jsx
export default function ProfilePage({ userId }) {
	return (
		<Profile
			userId={userId}
			key={userId}
		/>
	);
}

function Profile({ userId }) {
	// ✅ This and any other state below will reset on key change automatically  
	const [comment, setComment] = useState('');  
	// ...
}
```

일반적으로, React는 동일한 위치에 동일한 컴포넌트가 랜더링될 때 상태를 보존한다. **`userId`를 `Profile` 컴포넌트에 `key`로 전달함으로써, 당신은 React가 다른 `userId`를 가진 두 가지 `Profile` 컴포넌트를 어떠한 상태도 공유하지 않는 두 가지 다른 컴포넌트로 다루게 요구할 수 있다.**** (당신이 `userId`에 설정했던) key가 변경될 때마다, React는 DOM을 재생성하고 `Profile` 컴포넌트와 그것의 모든 자식들의 [상태를 초기화](https://react.dev/learn/preserving-and-resetting-state#option-2-resetting-state-with-a-key)할 것이다. 이제 `comment` 필드는 프로필 사이를 이동할 때 자동으로 지워질 것이다.

이 예제에서 다음을 주목하자. 오직 외부 `ProfilePage` 컴포넌트만이 exported 되고 프로젝트에 다른 파일에서 보여진다. `ProfilePage`를 랜더링하는 컴포넌트는 그것에 키를 전달할 필요가 없다: 그들은 일반적인 prop으로 `userId`를 전달한다. `ProfilePage`가 내부 `Profile` 컴포넌트에 `key`를 전달한다는 사실은 세부 구현사항이다.

## Adjusting some state when a prop changes
때때로, 당신은 prop 변화에 따른 상태의 일부분을 조정하거나 리셋하기를 원할 수 있다, 하지만 그것의 전부는 아니다.


## Summary
- **props 혹은 state를 기반으로 상태를 업데이트 하고 싶다면 Effect를 사용할 필요가 없다.** 이것은 불필요한 랜더링을 만들 뿐이다. 랜더링 동안에 새로운 값을 계산할 수 있게 만들면 이것은 자동으로 props 혹은 state의 변화에 따라 업데이트될 것이다.

- 이때 비싼 연산의 경우 `useMemo`를 이용하여 값을 **캐싱(메모이즈)** 할 수 있다.

- `console.time` / `console.timeEnd`를 통해 연산에 소요되는 시간을 계산하고 이를 통해 메모 사용에 대한 정보를 얻을 수 있다. (실행에 1ms 이상의 시간이 소요된다면 메모 사용을 고려하자.)

- 컴포넌트의 prop에 `key`를 전달함으로써 **컴포넌트의 상태를 초기화**할 수 있다.