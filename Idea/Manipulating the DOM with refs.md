React는 자동으로 render 출력에 맞게 DOM을 업데이트 한다. 그래서 컴포넌트는 그것을 제어하는 것을 필요로 하지 않는다. 하지만 때때로 React에 의해 관리되는 DOM 요소에 접근할 필요가 있을 수 있다. - 예를들어, **node를 포커스**하기 위해, 그것을 **스크롤** 하기 위해, 그것의 **사이즈나 위치를 측정**할 필요가 있을 수 있다. React에서 그것을 하기 위한 내장된 방법은 없다. 그렇기에 DOM node에 ref가 필요할 것이다. 

## Getting a ref to the node

```js
import { useRef } from 'react';
```

```jsx
const myRef = useRef(null);
```

```jsx
<div ref={myRef}>
```


```jsx
// You can use any browser APIs, for example:
myRef.current.scrollIntoView();
```

## Example: Focusing a text input

## Example: Scrolling to an element

> [!info] How to manage a list of refs using a ref callback
> 위의 예시에서 수 많은 refs가 미리 정의되어 있지만, 때때로 리스트에 있는 각각의 아이템에 대해 ref가 필요할 지도 모른며 얼마나 많은 ref를 가져야할 지 모를지도 모른다. 

## Accessing another component's DOM nodes
디자인 시스템에서 buttons, inputs 등과 같은 저수준의 컴포넌트에서 그들의 DOM 노드에 ref를 앞으로 하는 것은 흔한 패턴이다. 반면에 froms, lists 혹은 페이지 등과 같은 고수준의 컴포넌트는 보통 DOM 구조에 우연한 종속성을 피하기 위해 그들의 DOM node를 노출하지 않는다.

> [!info] Exposing a subset of the API with an imperative handle

## When React attaches the refs

> [!info] Flushing state updates synchronously with flushSync

## Best practices for DOM manipulation with refs
## Summary

- 예측할 수 없는 수 많은 ref를 다루기 위해 [ref callback](https://react.dev/reference/react-dom/components/common#ref-callback)을 사용할 수 있다.

- 기본적으로 다른 컴포넌트의 DOM 조작을 막기 위해 ref의 사용이 제한되어 있지만 `forwardRef`를 사용하면 다른 컴포넌트의 DOM에 접근할 수 있다.

- `useRef`를 사용한 DOM 요소 조작에서 제한적인 기능만을 노출시키고 싶다면 `useImperativeHandle`를 사용할 수 있다.

- *render phase* 에는 DOM이 아직 존재하지 않기 때문에 *commit phase*에 `ref`를 설정한다.

- `ref`를 이용하여 React가 관리하고 있는 DOM node를 조작하는 것은 피해야한다. 이것은 예측할 수 없는 충돌을 가져올 수 있으며 이것은 곧 일관되지 않은 시각적 결과를 초래할 수 있다.

- 보통 focusing, scrolling, 혹은 DOM 요소 measuring과 같은 비파괴적인 행위에 `ref`를 사용한다.





