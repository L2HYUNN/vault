몇몇 자바스크립트 함수는 순수 함수이다. 순수 함수(Pure function)는 오직 계산만을 수행하며 더 이상 아무것도 수행하지 않는다. 엄격하게 컴포넌트를 오직 순수 함수로만 작성함으로써, 코드베이스가 커짐에 따라 발생하는 당황스러운 버그들이나 예측할 수 없는 동작을 피할 수 있다.

이러한 장점들을 얻기 위해서는 따라야하는 몇 가지 규칙들이 존재한다.

> [!info] You will learn
> - **순수함(Purity)** 란 무엇인지 그리고 어떻게 순수함이 버그를 피하는데 도움이 되는지
> - 랜더 단계에서 변경을 피함으로써 컴포넌트를 순수하게 유지하는 방법
> - 컴포넌트에서 실수를 찾기 위해 **엄격 모드(Strict Mode)**를 사용하는 방법 

## Purity: Components as formulas
컴퓨터 과학(computer science)에서 (그리고 특히 함수형 프로그래밍의 세계에서) [순수 함수](https://wikipedia.org/wiki/Pure_function)는 다음의 특징을 가지는 함수를 의미한다:

- **순수함수는 자신의 일에만 신경쓴다.**  
  순수 함수가 불러지기 전에 존재했던 객체나 변수들을 변경시키지 않는다.
- **동일한 입력, 동일한 출력.**
  동일한 입력들이 주어지면, 순수 함수는 항상 동일한 결과를 반환해야만 한다.

당신은 이미 순수 함수의 하나의 예시인 수학에서의 공식에 익숙할 지 모른다.

수학 공식 `y = 2x`를 생각해보자.

만약 `x = 2` 일때 항상 `y = 4` 이다.

만약 `x = 3` 일때 항상 `y = 6` 이다.

만약 `x = 3` 일때 때로는 하루의 시간이나 스톡 마켓의 상태에 의존하여 9 또는 -1 또는 2.5가 되지는 않는다.

`y = 2x`에 대하여 `x = 3` 일때 **항상** `y = 6`이다.

만약 우리가 이것을 자바스크립트 함수로 만든다면, 그것은 다음과 같다:

```js
function double(number) {
	return 2 * number; 
}
```

> [!note]
> 위의 예시에서 `double`은 순수 함수이다. 만약 3을 입력하면 항상 6을 반환한다.

React는 이러한 개념 중심으로 고안되었다. **React는 당신이 작성하는 모든 컴포넌트를 순수 함수로 간주한다.** 이것은 당신이 작성한 React 컴포넌트가 항상 동일한 입력이 주어졌을 때 동일한 JSX를 반환한다는 것을 의미한다:

```jsx
function Recipe({ drinkers }) {
	return (
		<ol>
			<li>Boil {drinkers} cups of water.</li>
			<li>Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.</li>
			<li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
		</ol>
	);
}

  

export default function App() {
	return (
		<section>
			<h1>Spiced Chai Recipe</h1>
			<h2>For two</h2>
			<Recipe drinkers={2} />
			<h2>For a gathering</h2>
			<Recipe drinkers={4} />
		</section>
	);
}
```

`drinkers = {2}`를 `Recipe`에 전달할 때, 항상 `2 cups of water` 를 가지는 JSX를 반환한다.

만약 `drinkers = {4}`를 전달하면, 항상 `4 cups of water` 를 가지는 JSX를 반환한다.

이것은 마치 수학 공식과 같다.

> [!tip] You could think of your components as recipes
> 만약 레시피를 따르고 요리하는 과정에서 새로운 재료들을 섞지 않는다면, 매번 같은 요리를 얻을 것이다. 그 "요리"는 컴포넌트가 React에 제공하여 [랜더링](https://react.dev/learn/render-and-commit)하는 JSX이다.

## Side Effects: (un)intended consequences
React의 랜더링 과정은 항상 순수해야한다. 컴포넌트들은 오직 그들의 JSX 만을 반환해야하며, 랜더링 전에 존재했던 어떤 객체나 변수들을 변경시키면 안된다. - 그것은 그들을 `비순수`하게 만든다.

여기에 이 규칙을 따르지 않는 컴포넌트가 있다: 

```jsx 
let guest = 0;

function Cup() {
	// Bad: changing a preexisting variable!
	guest = guest + 1;
	return <h2>Tea cup for guest #{guest}</h2>
}

export default function TeaSet() {
	return (
		<>
			<Cup />
			<Cup />
			<Cup />
		</>
	);
}
```

이 컴포넌트는 컴포넌트 밖에 선언된 `guest` 변수를 읽고 쓴다. 이것은 이 **컴포넌트를 여러번 호출하는 것이 다른 JSX를 생성한다는 것**을 의미한다. 그리고 더 중요한 것은, 만약 다른 컴포넌트가 `guest`를 읽으면 그들은 랜더링된 시점에 따라 또 다른 JSX를 생성할 것이다. **그것은 예측할 수 없다.**

> [!summary]
> React의 컴포넌트는 순수 함수이다. 순수 함수로 컴포넌트를 만듬으로써 우리는 늘 동일한 JSX를 반환하는 예측 가능한 컴포넌트를 사용할 수 있다. 이것은 더욱 안전한 코드베이스를 유지 할 수 있게 만들어준다. 

우리의 공식 `y = 2x`로 돌아가보자, 이제 심지어 만약 `x = 2`라면, 우리는 `y = 4`를 신뢰할 수 없다. 우리의 테스트들이 실패할 수 있으며, 우리의 유저들은 당황할 수 있다. - 당신은 어떻게 이것이 혼란스러운 버그로 이어지는지 볼 수 있다.

당신은 [대신에 prop으로 `guest`를 전달함으로써](https://react.dev/learn/passing-props-to-a-component)이 컴포넌트를 고칠 수 있다:

```jsx
function Cup({ guest }) {
	return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
	return (
		<>
			<Cup guest={1} />
			<Cup guest={2} />
			<Cup guest={3} />
		</>
	);
}
```

이제 당신의 컴포넌트는 오직 `guest` prop에 의존하여 JSX를 반환하기 때문에 순수하다.

> 일반적으로, 컴포넌트가 특정한 순서를 가지고 랜더링될 거라고 예상해서는 안된다. 

`y = 2x`를 `y = 5x` 전에 호출하든 후에 호출하든 그것은 중요하지 않다: 두 공식은 서로 독립적으로 해결된다. 같은 방식으로 각각의 컴포넌트는 오직 **"그 자체로 생각"** 해야만 하며, 랜더링 중에 다른 컴포넌트에 함께 협력하려고 하거나 의존하려고 해서는 안된다. 랜더링은 마치 각각의 학교 시험과 같다. 각각의 컴포넌트는 그들 자신의 JSX만을 계산해야한다.

> [!example] DEEP DIVE
> #### Detecting impure calculations with StrictMode
> ---
> 아직 그들을 모두 사용해보지 않았을 수도 있지만, React에는 랜더링 중에 읽을 수 있는 세 가지 종류의 입력이 있다: [props](https://react.dev/learn/passing-props-to-a-component), [state](https://react.dev/learn/state-a-components-memory), 그리고 [context.](https://react.dev/learn/passing-data-deeply-with-context) 항상 이 입력들을 **읽기 전용(read-only)** 으로 다뤄야만 한다.
> 
> 유저의 입력에 대한 반응으로 무언가를 변경하고 싶을 때, 변수에 쓰기(writing)을 하는 대신에 [set state](https://react.dev/learn/state-a-components-memory)를 사용해야만 한다. 컴포넌트가 랜더링되는 동안 이미 존재하는 변수나 객체를 절대 변경해서는 안된다.
> 
> React는 "엄격 모드(Strict Mode)"를 제공하며 그것은 개발 중 각각의 컴포넌트 함수를 **두 번씩 호출**한다. **컴포넌트 함수를 두 번씩 호출함으로써, 엄격 모드는 이 규칙을 어기는 컴포넌트를 찾을 수 있게 도와준다.**
> 
> 원본 예시가 어떻게 "Guest #1", "Guest #2", 그리고 "Guest #3" 대신에 "Guest #2", "Guest #4", 그리고 "Guest #6"를 보여주는지에 주목하자. 원본 함수는 비순수 함수였기에, 두 번 호출하면 오류가 발생하였다. 하지만 수정된 순수 버전의 함수는 심지어 매번 함수를 두 번 호출하였어도 문제 없이 동작한다. **순수 함수는 오직 계산만을 수행 하기 때문에, 두 번 호출하여도 아무것도 변경되지 않는다.** -- 마치 `double(2)`을 두번 호출하여도 반환되는 것은 변하지 않는 것과 `y = 2x`를 두 번 풀어도 y의 값이 변화지 않는 것과 같다. 동일한 입력에는 항상 동일한 결과를 얻는다.
> 
> 엄격 모드는 배포(production) 단계에 영향을 끼치지 않기 때문에, 유저가 사용하는 app의 성능을 저하시키지 않는다. 엄격 모드를 설정하기 위해서는 루트 컴포넌트를 `<React.StrictMode>`로 감쌀 수 있다. 일부 프레임워크들은 이것을 기본값으로 사용한다.


> [!question]
> 리액트에서 순수 컴포넌트를 확인하기 위한 방법으로 컴포넌트 함수를 두 번 호출한다면, 일반적인 순수 함수를 체크하는 방법에도 동일한 방법을 적용할 수 있지 않을까? (사이드 이팩트 찾기)

## Local mutation: Your component's little secret
위의 예시에서, 문제는 랜더링 중에 컴포넌트가 이미 존재하는 변수를 변경한다는 것이었다. 이것은 종종 **"mutation"** 이라고 불리며 조금 두렵게 들릴 수 있다. 순수 함수들은 함수 범위의 밖에 있는 변수들 혹은 함수를 부르기 전에 만들어진 만들어진 객체들을 변경시키지 않는다. - 그것은 함수를 비순수하게 만든다.

그러나, **랜더링 동안 방금 생성된 변수나 객체를 변경하는 것은 완전히 상관없다.** 

예를들어, 하나의 `[]` 배열을 만들고, 그것을 `cups`라는 변수에 할당하고 그 안에 12개의 컵들을 `push` 한다:
```jsx
function Cup({ guest }) {
	return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
	let cups = [];
}
```

## Where you can cause side effects
함수형 프로그래밍은 순수함에 크게 의존하지만, 몇몇 포인트, 어딘가, 무언가는 변해야만 한다. 그것은 프로그래밍의 핵심이다. 이것들은 변화시키며 - 화면 업데이트, 애니메이션 시작, 데이터 변경 -  **side effects**라고 불린다. 

React에서 **side effects는 항상 [event handlers](https://react.dev/learn/responding-to-events)에 속해있다.** 비록 event handlers가 컴포넌트 안에 정의되어 있을지라도, 그들은 랜더링 중에 실행되면 안된다. **그렇기에 event handlers는 순수할 필요가 없다.**

> [!info] Why does React care about Purity?
> 순수 함수를 작성하는 것은 일부의 습관과 규율을 가진다. 하지만 이것은 놀라운 기회를 풀어준다:
> 
> - 컴포넌트를 다른 환경에서 실행할 수 있다.
> - input이 변경되지 않은 컴포넌트의 [랜더링을 스킵](https://react.dev/reference/react/memo)함으로써 성능을 향상시킬 수 있다. 이것은 순수 함수가 항상 동일한 결과를 반환하기 때문에 안전하다. 그래서 그들은 cache에 안전하다.
> - 깊은 컴포넌트 트리를 랜더링 하는 도중에 일부 데이터가 변경되는 경우, React는 그밖의 render를 마무리하는 것에 시간을 낭비하지 않고 바로 rendering을 재시작한다. 순수함은 언제든 계산을 멈출 수 있어 그것을 안전하게 만든다.

## Recap
## Summary
- React의 컴포넌트는 순수해야만한다. 순수한 컴포넌트는 항상 동일한 JSX를 반환하기 때문에 예측 가능한 컴포넌트를 사용할 수 있다. 

- Rendering은 언제나 발생할 수 있기 때문에, 컴포넌트들은 랜더링 과정에 서로 의존하지 않아야 한다.

- Rendering에 컴포넌트가 사용하는 어떤 input도 변경하지 말아야 한다. 그것은 props, state, 그리고 context를 포함한다. 화면을 업데이트 하기 위해서는 이전에 존재하는 객체를 변경하는 대신에 `"set" state`를 사용해야 한다.

- 반환하는 JSX에 컴포넌트의 논리를 표현하기 위해 노력하자. "무언가를 변경하는 것"이 필요한 경우 event handler를 통해 그것을 보통 실행할 수 있다. 마지막 수단으로 `useEffect`를 사용할 수 있다.

- React에서 순수한 컴포넌트를 사용하는 것은 컴포넌트를 다른 환경에서 실행할 수 있게 해주며, 랜더링을 스킵할 수 있게 만들어준다. (항상 동일한 결과를 반환하기 때문에 cache에 안전하다)











