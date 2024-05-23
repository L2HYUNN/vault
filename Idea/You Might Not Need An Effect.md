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

기존에 