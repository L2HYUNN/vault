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

`List` 컴포넌트는 prop으로 `items` 리스트를 받는다 그리고 `selection` 상태 변수에 있는 선택된 아이템을 유지한다. 당신은 `items` prop이 다른 배열을 받을 때마다 `selection` 변수를 `null`로 초기화 하기를 원한다:

```jsx
function List({ items }) {
	const [isReverse, setIsReverse] = useState(false);
	const [selection, setSelection] = useState(null);

	// 🔴 Avoid: Adjusting state on prop change in an Effect  
	useEffect(() => {  
		setSelection(null);  
	}, [items]);  
	// ...
}
```

이것은 이상적이지 않다. `items`이 변경될 때마다, `List` 그리고 그것의 자식 컴포넌트들은 먼저 오래된 `selection` 변수를 가지고 랜더링될 것이다. 이후에 React는 DOM을 업데이트하고 Effect를 실행할 것이다. 마지막으로, `setSelection(null)` 호출은 `List`  그리고 그것의 자식 컴포넌트들에 또 다른 리랜더링을 야기할 것이다, 이 전체 과정이 다시 재시작된다.

Effect를 제거하는 것으로부터 시작하자, 랜더링동안 직접 상태를 조정하라:

```jsx
function List({ items }) {
	const [isReverse, setIsReverse] = useState(false);
	const [selection, setSelection] = useState(null);

	// Better: Adjust the state while rendering  
	const [prevItems, setPrevItems] = useState(items);  
	
	if (items !== prevItems) {  
		setPrevItems(items);  
		setSelection(null);  
	}  
	// ...
}
```

이와 같이 [이전 랜더링으로부터 정보를 저장](https://react.dev/reference/react/useState#storing-information-from-previous-renders)하는 것은 이해하기 어려울 수 있지만, Effect에서 동일한 상태를 업데이트 하는 것보다는 낫다. 위의 예시에서 `setSelection`은 랜더링 동안 직접적으로 호출된다. React는 `return` 문을 빠져나온 즉시 `List` 컴포넌트를 리랜더링할 것이다. React는 아직 `List` 컴포넌트의 자식을 랜더링하거나 DOM을 업데이트 하지 않는다, 그렇기에 이것은 `List` 컴포넌트의 자식들이 오래된 `selection`
변수를 랜더링하는 것을 스킵할 수 있게 해준다. 

랜더링 중에 컴포넌트를 업데이트할 때, React는 JSX 반환을 내던지고 즉시 랜더링을 재시도한다. 매우 느린 계단식 재시도를 피하기 위해, React는 오직 랜더링동안 같은 컴포넌트의 상태만을 업데이트 할 수 있게 허락한다. 만약 랜더링 도중 또 다른 컴포넌트의 상태를 업데이트하려 한다면, 에러를 보게될 것이다. `items !== prevItems`와 같은 조건문은 반복을 피하기 위해 필요하다. 이와 같이 상태를 조정할 수 있지만, DOM을 변경하거나 타임아웃을 설정하는 것과 같은 다른 사이드 이팩트는 [컴포넌트를 순수하게 유지](https://react.dev/learn/keeping-components-pure)하기 위해 이벤트 핸들러 혹은 Effect에 존재해야만 한다.

**비록 이 패턴이 Effect를 사용하는 것 보다 조금 더 효율적일지라도, 대부분의 컴포넌트에는 그것이 필요하지 않을 것이다.** 어떻게 그것을 행하던지 간에, props 혹은 다른 상태에 기반하여 상태를 조정하는 것은 당신의 데이터 흐름을 이해하고 디버깅하기 어렵게 만들 것이다. 항상 대신에 [키를 이용하여 모든 상태를 초기화](https://react.dev/learn/you-might-not-need-an-effect#resetting-all-state-when-a-prop-changes) 할 수 있는지 혹은 [랜더링중에 모든것을 계산](https://react.dev/learn/you-might-not-need-an-effect#updating-state-based-on-props-or-state)할 수 있는지 확인하라. 예를 들어, 선택된 아이템을 저장(그리고 리셋)하는 것 대신에 당신은 선택된 아이템의 ID를 저장할 수 있다:

```jsx
function List({ items }) {
	const [isReverse, setIsReverse] = useState(false);
	const [selection, setSelection] = useState(null);

	// ✅ Best: Calculate everything during rendering  
	const selection = items.find(item => item.id === selectedId) ?? null;  
	// ...
}
```

이제 상태를 "조정"하는 것이 전혀 필요하지 않다. 만약 선택된 ID를 가진 아이템이 리스트에 있다면, 그것은 선택된채로 남는다. 만약 그것이 아니라면, 랜더링 중에 계산된 `selection`은 `null`일 것이다 왜냐하면 매칭된 아이템을 찾을 수 없기 때문이다. 이 동작은 다르지만 선택을 유지하는 `items`에 대한 대부분의 변경이 더 나은 것으로 볼 수 있다.

## Sharing logic between event handlers
제품을 구매할 수 있게 해주는 두 가지 버튼 (Buy and Checkout)을 가진 제품 페이지를 가지고 있다고 해보자. 당신은 유저가 카트에 제품을 넣을때마다 알림을 보여주기를 원한다. 두 버튼의 클릭 핸들러에서 `showNotification()` 함수를 호출하는 것이 반복적인 작업처럼 느껴질 수 있다. 그렇기에 당신은 Effect에 이 로직을 위치하고 싶은 욕구를 느낄지도 모른다:

```jsx
function ProductPage({ product, addToCart }) {
	// 🔴 Avoid: Event-specific logic inside an Effect
	useEffect(() => {
		if (product.isInCart) {
			showNotification(`Added ${product.name} to the shopping cart!`);
		}
	}, [product]);

	function handleBuyClick() {
		addToCart(product);
	}

	function handleCheckoutClick() {
		addToCart(product);
		navigateTo('/checkout');
	}
}
```

이 Effect는 불필요하다. 그것은 또한 버그를 발생시킬 가능성이 높다. 예를들어, 당신의 app이 페이지 리로드 사이에 쇼핑 카트를 "기억"하고 있다고 해보자. 만약 당신이 상품을 카트에 추가하고 페이지를 새로고침하면, 알림은 다시 나타날 것이다. 이것은 `product.isIncart`가 이미 페이지를 로드할 때 `true`이기 때문에, 위의 Effect는 `showNotification()`을 호출할 것이다.

**당신이 일부 코드가 Effect에 있어야하는지 혹은 이벤트 핸들러에 있어야 하는지 확신하지 못할 때, 왜 이 코드를 실행해야 하는지 스스로에게 물어보아라. 오직 컴포넌트가 유저에게 보여졌기 때문에 코드를 실행해야만 할 때 Effect를 사용해라.** 예를 들어, 알림이 유저가 버튼을 눌렀기 때문에 보여야만 한다면, 그 페이지가 보여졌기 때문이 아닌 것이다. Effect를 제거하고 두 이벤트 핸들러에서 호출되는 함수 안에 공유되는 로직을 넣아라.

```jsx
function ProductPage({ product, addToCart }) {
	// ✅ Good: Event-specific logic is called from event handlers  
	function buyProduct() {  
		addToCart(product);  
		showNotification(`Added ${product.name} to the shopping cart!`);  
	}  
	
	function handleBuyClick() {  
		buyProduct();  
	}  
	
	function handleCheckoutClick() {  
		buyProduct();  
		navigateTo('/checkout');  
	
	}  
	// ...
}
```

이것은 둘 다 불필요한 Effect를 제거하고 버그를 고친다.

## Sending a POST request
`Form` 컴포넌트는 두 종류의 POST 요청을 보낸다. 그것은 마운트 되었을 때 이벤트 분석을 보낸다. 폼을 채우고 제출 버튼을 클릭할 때, 그것은 `/api/register` 엔드포인트에 POST 요청을 보낼 것이다: 

```jsx
function Form() {
	const [firstName, setFirstName] = useState('');
	const [lastName, setLastName] = useState('');

	// ✅ Good: This logic should run because the component was displayed  
	useEffect(() => {  
		post('/analytics/event', { eventName: 'visit_form' });  
	}, []);  
	  
	// 🔴 Avoid: Event-specific logic inside an Effect  
	const [jsonToSubmit, setJsonToSubmit] = useState(null);  
	
	useEffect(() => {  
		if (jsonToSubmit !== null) {  
			post('/api/register', jsonToSubmit);  
		}  
	}, [jsonToSubmit]);  
	
	function handleSubmit(e) {  
		e.preventDefault();  
		setJsonToSubmit({ firstName, lastName });  
	}  
	// ...
}
```

앞의 예와 동일한 기준을 적용해보자.

분석 POST 요청은 Effect에 있어야만한다. 이것은 분석 이벤트를 보내는 이유가 폼이 보여졌기 때문이다. (이것은 개발 모드에서 두 번 실행된다, 하지만 이것을 다루는 방법은 [여기](https://react.dev/learn/synchronizing-with-effects#sending-analytics)를 참조하라.)

그러나, `/api/register` POST 요청은 폼이 보여지는 것에 의해 야기되는 것이 아니다. 당신은 오직 특정한 순간: 유저가 버튼을 눌렀을때 그 요청을 보내기를 원한다. 그것은 오직 특별한 상호작용에서만 일어나야 한다. 두 번째 Effect를 제거하고 이벤트 핸들러에 POST 요청을 옮겨라:

```jsx
function Form() {
	const [firstName, setFirstName] = useState('');
	const [lastName, setLastName] = useState('');

	// ✅ Good: This logic should run because the component was displayed  
	useEffect(() => {  
		post('/analytics/event', { eventName: 'visit_form' });  
	}, []);  
	
	function handleSubmit(e) {  
		e.preventDefault();  
		// ✅ Good: Event-specific logic is in the event handler
		post('/api/register', { firstName, lastName });  
	}  
	// ...
}
```

이벤트 핸들러 혹은 Effect에 일부 로직을 넣을지 말지 선택할 때, 답하기 위해 필요한 주요 질문은 유저의 관점에서 그것이 무슨 종류의 로직인지 아는 것이다. 만약 이 로직이 특정한 상호작용에 의해서 발생한다면, 이벤트 핸들러에 그것을 두어야 한다. 만약 그것이 유저가 스크린에 컴포넌트를 보는것에 의해 발생했다면, Effect에 그것을 두어야 한다.

## Chains of computations
때떄로 다른 상태에 근거하여 각 상태의 일부를 조정하는 Effect를 체인하고 싶은 욕구가 생길 수 있다:  

```jsx
function Game() {
	const [card, setCard] = useState(null); 
	const [goldCardCount, setGoldCardCount] = useState(0);  
	const [round, setRound] = useState(1);  
	const [isGameOver, setIsGameOver] = useState(false);
	
	// 🔴 Avoid: Chains of Effects that adjust the state solely to trigger each other  
	useEffect(() => {  
		if (card !== null && card.gold) {  
			setGoldCardCount(c => c + 1);  
		}  
	}, [card]);  
	
	useEffect(() => {  
		if (goldCardCount > 3) {  
			setRound(r => r + 1); 
			setGoldCardCount(0);  
		}  
	}, [goldCardCount]);  
	
	useEffect(() => {  
		if (round > 5) {  
			setIsGameOver(true);  
		}  
	}, [round]);  

	useEffect(() => {  
		alert('Good game!');  
	}, [isGameOver]);  
	
	function handlePlaceCard(nextCard) {  
		if (isGameOver) {  
			throw Error('Game already ended.');  
		} else {  
			setCard(nextCard);  
		}  
	}  
	// ...
}
```

이 코드에는 두 가지 문제가 있다.

한 가지 문제는 그것이 매우 비효율적이라는 것이다: 컴포넌트는 (그리고 그것의 자식들은) 체인에 있는 각 `set` 호출 사이에 리랜더링을 가진다. 위의 예시에서, 최악인 케이스(`setCard` -> render -> `setGoldCardCount` -> render -> `setRound` -> render -> `setIsGameOver` -> render)는 트리 아래에 세 가지 불필요한 리랜더링이 있다는 것 이다.

심지어 그것이 느리지 않더라도, 당신의 코드가 발전함에 따라, 당신이 사용한 "체인"이 새로운 요구사항에 맞지 않는 케이스가 발생할 것이다. 게임 동작에 대한 기록을 단계별로 살펴보는 방법을 추가한다고 상상해보자. 당신은 각 상태 변수를 과거의 값으로 업데이트함으로써 그것을 하려 했을 것이다. 그러나 `card` 상태를 과거의 값으로 설정하는 것은 Effect 체인을 다시 트리거할 것이고 당신이 보고있는 데이터를 변경할 것이다. 이와 같은 코드는 경직되고 취약한 경우가 많다.

이 경우에, 랜더링 중에 가능한 것을 계산하고 이벤트 핸들러에서 상태를 조정하는 것이 더 낫다.

```jsx
function Game() {
	const [card, setCard] = useState(null); 
	const [goldCardCount, setGoldCardCount] = useState(0);  
	const [round, setRound] = useState(1);  
	const [isGameOver, setIsGameOver] = useState(false);
	
	// ✅ Calculate what you can during rendering  
	const isGameOver = round > 5;
	
	function handlePlaceCard(nextCard) {  
		if (isGameOver) {  
			throw Error('Game already ended.');  
		} 

		// ✅ Calculate all the next state in the event handler  
		setCard(nextCard);  
		if (nextCard.gold) {  
			if (goldCardCount <= 3) {  
				setGoldCardCount(goldCardCount + 1);  
			} else {  
				setGoldCardCount(0);  
				setRound(round + 1);  
				if (round === 5) {  
				alert('Good game!');  
				}  
			}  
		}
	}  
	// ...
}
```

이것이 훨씬 더 효율적이다. 또한, 만약 게임 기록을 보기위한 방법을 구현 한다면, 이제 당신은 모든 다른 값을 조정하는 Effect 체인을 트리거시키지 않고 각 상태 변수를 과거의 움직임으로 설정 할 수 있다. 만약 여러 이벤트 핸들러에서 로직을 재사용 해야 한다면, 당신은 [함수를 추출](https://react.dev/learn/you-might-not-need-an-effect#sharing-logic-between-event-handlers)하고 핸들러에서 그것을 호출할 수 있다.

이벤트 핸들러 내부에서, [상태는 마치 스냅샷처럼 동작](https://react.dev/learn/state-as-a-snapshot)한다는 것을 기억하라. 예를 들어, 심지어 `setRound(round + 1)`을 호출한 후에, `round` 변수는 버튼을 클릭했던 순간의 변수를 반영할 것이다. 만약 당신이 연산에 다음 변수를 사용해야만 한다면, 수동적으로 `const nextRound = round + 1`과 같이 선언하라.

일부 경우에, 이벤트 핸들러에서 직접적으로 다음 상태를 계산할 수 없을 수 있다. 예를 들어, 다음 드랍다운의 옵션이 이전의 드랍다운의 변수에 의해 결정되는 다수의 드랍다운을 가진 폼을 상상해보자. 이후에, Effect의 체인은 네트워크와 동기화하기 때문에 적절하다.

## Initializing the application
일부 로직은 app이 로드될 때 단 한 번만 실행되어야 한다.

최상위 수준의 컴포넌트에 있는 Effect에 그것을 위치하고 싶다고 생각해보자:

```jsx
function App() {
	// 🔴 Avoid: Effects with logic that should only ever run once  
	useEffect(() => {  
		loadDataFromLocalStorage();  
		checkAuthToken();  
	}, []);  
	// ...
}
```

그러나 당신은 빠르게 그것이 [개발 모드에서 두 번 실행](https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)된다는 것을 발견하게 될 것이다. 이것은 문제를 야기할 수 있다 - 예를 들어, 아마도 그것은 인증 토큰을 무효화 한다. 왜냐하면 그 함수는 두 번 호출되게 디자인되지 않았기 때문이다. 일반적으로 당신의 컴포넌트는 다시 마운트 되었을 때 탄력이 있다. 이것은 당신의 최상단 `App` 컴포넌트를 포함한다.

비록 실제로 프로덕션 모드에서 그것이 다시 마운트 될 일이 없다 하더라도, 모든 컴포넌트에서 동일한 제약을 따르는 것이 코드를 옮기고 재사용하기 쉬울 것이다. 만약 일부 로직이 컴포넌트가 마운트되었을 때 보다 앱이 로드 되었을 때 한 번 실행되어야만 한다면, 그것이 이미 실행되었는지 아닌지 추적하기 위해 상단에 변수를 추가하라:

```jsx
let didInit = false;

function App() {
	useEffect(() => {
		if (!didInit) {
			didInit = true;
			// ✅ Only runs once per app load
			loadDataFromLocalStorage();
			checkAuthToken();
		}
	}, []);
	// ...
}
```

당신은 그것을 모듈이 초기화 되거나 앱을 랜더하기 전에 또한 실행할 수 있다:

```jsx
if (typeof window !== 'undefined') { // Check if we're running in the browser.
	// ✅ Only runs once per app load  
	checkAuthToken();  
	loadDataFromLocalStorage();
}
```

최상위에 있는 코드는 당신의 컴포넌트가 임포트되었을 때 한 번 실행된다 - 심지어 랜더링이 끝나지 않더라도. 임의의 컴포넌트를 임포트 할 때 느려지거나 놀라운 동작을 피하기 위해, 이 패턴을 과다 사용하지 말아라. 앱 전역에 초기화 로직을 `App.js`와 같은 루트 컴포넌트 모듈 혹은 당신의 어플리케이션의 엔트리 포인트에 위치시켜라.

## Notifying parent components about state changes
내부에 `true` 혹은 `false`일 수 있는 `isOn` 상태를 가진 `Toggle` 컴포넌트를 작성한다고 해보자. 그것을 토글하는 몇 가지 다른 방법들이 있다 (클릭 혹은 드래그를 통해). 당신은 `Toggle`의 내부 상태가 변경될 때마다 부모 컴포넌트에 알리기를 원한다. 그래서 당신은 `onChange` 이벤트를 노출시키고 Effect에서 그것을 호출한다:

```jsx
function Toggle({ onChange }) {
	const [isOn, setIsOn] = useState(false);

	// 🔴 Avoid: The onChange handler runs too late  
	useEffect(() => {  
		onChange(isOn);  
	}, [isOn, onChange])

	function handleClick() {
		setIsOn(!isOn);
	}

	function handleDragEnd(e) {
		if (isCloserToRightEdge(e)) {
			setIsOn(true);
		} else {
			setIsOn(false);
		}
	}

	// ... 
}
```

처음과 같이, 이것은 이상적이지 않다. `Toggle`은 그것의 상태를 먼저 업데이트 하고 React는 화면을 업데이트 한다. 이후에 React는 부모 컴포넌트로부터 온 `onChange` 함수를 호출하는 Effect를 실행한다. 이제 부모 컴포넌트는 그것의 상태를 업데이트 할 것이고, 또 다른 랜더 패스를 시작할 것이다. 그것은 하나의 패스에서 모든 것을 실행하는 것이 더 낫다.

Effect를 제거하고 대신에 동일한 이벤트 핸들러에서 두 컴포넌트의 상태를 업데이트 하라:

```jsx
function Toggle({ onChange }) {
	const [isOn, setIsOn] = useState(false);

	function updateToggle(nextIsOn) {
		// ✅ Good: Perform all updates during the event that caused them
		setIsOn(nextIsOn);
		onChange(nextIsOn);
	}

	function handleClick() {
		updateToggle(!isOn);
	}

	function handleDragEnd(e) {
		if (isCloserToRightEdge(e)) {
			updateToggle(true);
		} else {
			updateToggle(false);
		}
	}
}
```

이 접근 방법을 가지고, `Toggle` 컴포넌트와 그것의 부모 컴포넌트는 이벤트 동안 그들의 상태를 업데이트 한다. React는 다른 컴포넌트로 부터 함께 [일괄 업데이트](https://react.dev/learn/queueing-a-series-of-state-updates) 한다, 그렇기에 오직 한 번의 랜더 패스가 존재할 것이다.

당신은 또한 상태를 함께 제거할 수 있을 것이다, 그리고 대신에 부모 컴포넌트로 부터 `isOn`을 받는다:

```jsx
// ✅ Also good: the component is fully controlled by its parent
function Toggle({ isOn, onChange }) {
	function handleClick() {
		onChange(!isOn);
	}

	function handleDragEnd(e) {
		if (isCloserToRightEdge(e)) {
			onChange(true);
		} else {
			onChange(false);
		}
	}

	// ...
}
```

["상태 끌어 올리기"](https://react.dev/learn/sharing-state-between-components)는 부모 컴포넌트가 부모 고유의 상태를 토글링 함으로써 `Toggle`을 완전히 제어하게 만든다. 이것은 부모 컴포넌트가 더 많은 로직을 포함해야만 한다는 것을 의미한다, 하지만 전반적으로 걱정할 상태는 줄어들 것이다. 두 가지 다른 상태 변수를 동기화하려고 할 때마다, 대신에 상태 끌어 올리기를 시도하라.

## Passing data to the parent
`Child` 컴포넌트는 일부 데이터를 패치하고 그것을 Effect에서 `Parent` 컴포넌트에 전달하고 있다:

```jsx
function Parent() {
	const [data, setData] = useState(null);
	//...
	return <Child onFetched={setData} />;
}

function Child({ onFetched }) {
	const data = useSomeAPI();
	// 🔴 Avoid: Passing data to the parent in an Effect
	useEffect(() => {
		if (data) {
			onFetched(data);
		}
	}, [onFetched, data]);
}
```

React에서, 데이터는 부모 컴포넌트에서 그들의 자식으로 흐른다. 화면에서 무언가 문제가 발생하였을 때, 당신은 어떤 컴포넌트가 이상한 prop을 전달했는지 혹인 이상한 상태를 가지는지 찾을때까지 컴포넌트 체인을 올라가며 정보가 어디로부터 왔는지 추적할 수 있다. 자식 컴포넌트가 Effect에서 부모 컴포넌트의 상태를 업데이트 할 때, 데이터 흐름은 매우 추적하기 어려워진다. 부모와 자식 둘 다 동일한 데이터가 필요하기 때문에, 부모 컴포넌트가 데이터를 패치하고 그것을 대신에 자식에게 전달해주게 만들자: 

```jsx
function Parent() {
	const data = useSomeAPI();
	// ...
	// ✅ Good: Passing data down to the child
	return <Child data={data} />;
}

function Child({ data }) {
	// ...
}
```

이것은 더욱 간단하고 데이터 흐름을 예측할 수 있게 만든다: 데이터는 부모에서 자식으로 흐른다.

## Subscribing to an external store
때때로, 당신의 컴포넌트들은 React 상태 외부의 일부 데이터를 구독해야할 필요가 있다. 이 데이터는 서드 파티 라이브러리 혹은 빌트인 브라우저 API로 부터 올 수 있다. 이 데이터는 React의 지식 없이 변경될 수 있기 때문에, 당신은 수동적으로 그것에 컴포넌트를 구독해야만한다. 이것은 종종 Effect를 가지고 행해진다, 예는 다음과 같다: 

```jsx
function useOnlineStatus() {
	// Not ideal: Manual store subscription in an Effect
	const [isOnline, setIsOnline] = useState(true);

	useEffect(() => {
		function updateState() {
			setIsOnline(navigator.onLine);
		}

		updateState();

		window.addEventListener('online', updateState);  
		window.addEventListener('offline', updateState);  
		return () => {  
			window.removeEventListener('online', updateState);  
			window.removeEventListener('offline', updateState);  
		};  
	}, []);  

	return isOnline;
}

function ChatIndicator() {
	cosnt isOnline = useOnlineStatus();
	// ...
}

```

여기에, 외부 데이터 스토어 (이 경우, 브라우저 `navigator.onLine` API)를 구독하는 컴포넌트가 있다. 이 API는 서버에 존재하지 않기 때문에 (그래서 그것은 초기 HTML에 사용될 수 없다), 초기에 그 상태는 `true` 이다. 브라우저에서 데이터 스토어의 값이 변경될 때마다, 컴포넌트는 그것의 상태를 업데이트한다.

비록 이것을 위해 Effect를 사용하는 것이 흔한 경우이더라도, React는 대신 선호되는 외부 저장소를 구독하기 위한 특별한 목적으로 제작된 Hook을 가지고 있다. Effect를 제거하고 [`useSyncExternalStore`](https://react.dev/reference/react/useSyncExternalStore)를 호출하는 것으로 변경하라:


## Summary
- **props 혹은 state를 기반으로 상태를 업데이트 하고 싶다면 Effect를 사용할 필요가 없다.** 이것은 불필요한 랜더링을 만들 뿐이다. 랜더링 동안에 새로운 값을 계산할 수 있게 만들면 이것은 자동으로 props 혹은 state의 변화에 따라 업데이트될 것이다.

- 이때 비싼 연산의 경우 `useMemo`를 이용하여 값을 **캐싱(메모이즈)** 할 수 있다.

- `console.time` / `console.timeEnd`를 통해 연산에 소요되는 시간을 계산하고 이를 통해 메모 사용에 대한 정보를 얻을 수 있다. (실행에 1ms 이상의 시간이 소요된다면 메모 사용을 고려하자.)

- 컴포넌트의 prop에 `key`를 전달함으로써 **컴포넌트의 상태를 초기화**할 수 있다.

- **React는 랜더링 중에 같은 컴포넌트의 상태만을 업데이트할 수 있다.** 이것은 매우 느린 계단식 재시도를 피할 수 있게 만들어준다. 이와 같은 랜더링 중 업데이트는 자식에 전파되기 전, DOM에 반영되기 전에 리랜더링을 실행한다.

- **이벤트와 관련된 로직을 이벤트 핸들러가 아닌 Effect에서 처리하지 말아라.**

- **이팩트 체인(Effect Chains)을 만드는 것은 굉장히 비효율적이다.** 가능한 랜더링 중에 가능한 것을 계산하고 이벤트 핸들러에서 상태를 조정하라.

- Effect를 통해 마운트 때마다 실행하는 것이 아닌 초기에 앱이 실행되었을 때 한 번 실행하고 싶다면 전역 플래그를 설정하여 실행 여부를 결정할 수 있다.

- **React에서 데이터는 부모에서 자식으로 단방향으로 이동한다.** 만약 자식에서 부모의 데이터를 업데이트 한다면 이것은 데이터 흐름을 추적하기 어렵게 만들 것이다. 데이터는 부모 컴포넌트에서 자식 컴포넌트로 흐르는 것이 이상적이다. 