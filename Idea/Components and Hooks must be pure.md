순수 함수는 오직 계산만을 수행하며 다른 것을 수행하지 않는다. 이것은 코드를 이해하고 디버그 하기 쉽게 만들며 React가 자동으로 Components와 Hooks을 올바르게 최적화 시킬 수 있게 해준다.

> [!note]
> 이 참조 페이지는 심화된 주제를 다루고 있으며 [Keeping Components Pure](https://react.dev/learn/keeping-components-pure) 페이지에서 다루는 컨셉에 익숙함을 필요로 한다.

## Why does purity matter?
React를 만드는 주요한 컨셉 중 하나는, React는 순수하다는 것이다. 순수 컴포넌트 혹은 훅은 다음과 같다:

- **항등성(Idempotent)**
- **랜더링(render)은 부수 효과(side effects)를 가지지 않는다**
- **지역 변수가 아닌것을 변경해서는 안된다**

랜더링(render)가 순수함을 유지할 때, React는 유저가 처음으로 보는 어떤 업데이트가 가장 중요한지 우선순위를 정하는 방법을 이해할 수 있다.

### How does React run your code?

> [!info] How to tell if code runs in render

## Components and Hooks must be idempotent

## Side effects must run outside of render

### When is it okay to have mutation?

## Props and state are immutable

### Don't mutate Props

### Don't mutate State

## Return values and arguments to Hooks are immutable

## Values are immutable after being passed to JSX