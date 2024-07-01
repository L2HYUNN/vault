원문: https://ui.toast.com/posts/ko_20210812


## Zustand?
Zustand는 독일어로 '상태'라는 뜻을 가진 라이브러리이며 Jotai를 만든 [카토 다이시](https://twitter.com/dai_shi)가 제작에 참여하고 적극적으로 관리하는 라이브러리이다. 아래의 특징을 가지고 있다.

- 특정 라이브러리에 엮이지 않는다. (그래도 React와 함께 쓸 수 있는 API는 기본적으로 제공한다.)
- 한 개의 중앙에 집중된 형식의 스토어 구조를 활용하면서, 상태를 정의하고 사용하는 방법이 단순하다.
- [Context API](https://ko.reactjs.org/docs/hooks-reference.html#usecontext)를 사용할 때와 달리 상태 변경 시 불필요한 리랜더링을 일으키지 않도록 제어하기 쉽다.
- React에 직접적으로 의존하지 않기 때문에 자주 바뀌는 상태를 직접 제어할 수 있는 방법도 제공한다. [(Transient Update라고 한다.)](https://github.com/pmndrs/zustand#transient-updates-for-often-occuring-state-changes)
- 동작을 이해하기 위해 알아야 하는 코드 양이 아주 적다. 핵심 로직의 코드 줄 수가 약 42줄밖에 되지 않는다. (VanillaJS 기준)

## 왜 Context API를 사용하지 않는가?
Context는 컴포넌트에 의존성을 주입할 수 있는 아주 효과적인 방법 중 하나이다. 하지만 부모 컴포넌트 쪽에 `Context.Provider` 컴포넌트를 선언하고 Context로 전달되는 값이 변경될 때 해당 Context를 사용하는 모든 자손 컴포넌트는 리랜더링된다.

다음과 같이 임의의 값과 값을 변경하는 방법을 제공하는 Context와 Provider를 만들었다고 가정해보자.

```js
const SomeObjectContext = React.createContext({ input: '', count: 0 });
const SetSomeObjectConteext = React.createContext();

const Provider = ({ children }) => {
  const [someObj, setSomeObj] = React.useState({ input: '', count: 0 });

  return (
    <SetSomeObjectConteext.Provider value={setSomeObj}>
      <SomeObjectContext.Provider value={someObj}>
        {children}
      </SomeObjectContext.Provider>
    </SetSomeObjectConteext.Provider>
  )
};
```

이 Context를 소비하는 컴포넌트가 여러 단계 아래쪽 자손일 수 있다.

```js
const InputConsumer = () => {
  const { input } = useContext(SomeObjectContext);
  // ...
}

const CountConsumer = () => {
  const { count } = useContext(SomeObjectContext);
  // ...
}

const App = () => (
  <Provider>
    <DeepChildren>
      <SomeOtherChildren>
        <AnotherChildren>
          {/* input 값만 사용하려 함 */}
          <InputConsumer />
        </AnotherChildren>
        {/* count 값만 사용하려 함 */}
        <CountConsumer />
      </SomeOtherChildren>
    </DeepChildren>
    <IDontCareContextChild />
    {/* SomeObject 값을 비꾸는 역할을 함 */}
    <ContextSetter />
  </Provider>
)
```

이렇게 선언된 컴포넌트 트리의 경우 `ContextSetter` 컴포넌트에서 Context 값의 일부만 바꾸는 동작을 실행하더라도, `InputConsumer`, `CounterConsumer` 모두 리랜더링이 일어나게 된다. 결국 객체 형태로 Context를 관리하면서 Context를 소비하는 컴포넌트가 많아질 경우 불필요한 리랜더링이 많이 일어나 애플리케이션의 성능 문제가 생길 수 있다.

이 문제를 해결하기 위한 방안은 [여러 가지](https://github.com/facebook/react/issues/15156#issuecomment-474590693)가 있다.

1. 하나의 거대한 값을 가진 Context를 만들지 말고 여럿으로 분리하여 필요한 부분만 사용하기
2. 컴포넌트를 쪼개고 `React.memo` 를 활용하기
3. `useMemo` 훅을 사용하여 컴포넌트 랜더링 부분을 감싸기

제일 권장되는 방법은 1번이지만, 여러 Context를 만들어 Provider로 주입할 때 [Provider Hell](https://dev.to/alfredosalzillo/the-react-context-hell-7p4)이라 불리는 중첩 Provider로 인한 가독성 문제가 생긴다. 또한 애플리케이션 규모나 구조에 따라 다르지만 캘린더는 일원화된 상태 스토어를 사용하는게 더 효과적일 것이라 판단했다.