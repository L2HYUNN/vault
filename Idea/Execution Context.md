**코드의 실행환경에 대한 여러가지 정보를 담고 있는 개념**, 자바스크립트 엔진에 의해 만들어지고 사용 되는 코드 정보를 담은 객체의 집합

> [!Info]
> **실행 컨텍스트**는 자바스크립트 코드가 실행되고 코드를 실행하는데 필요한 필수적인 정보를 제공하고 변수, 함수 그리고 프로그램의 현재 상태를 포함하는 필수적인 정보를 전달한다.

## Scope
자바스크립트의 코드는 다음과 같이 3가지 종류로 볼 수 있다.

1. Global Scope (code)
2. Function Scope (code)
3. `eval()` Scope (code)

> 이들은 각각 자신만의 실행 컨텍스트를 생성한다.

### Global Execution Context, GEC
자바스크립트 파일을 실행하기 전에 생성되는 전역 실행 컨텍스트
### Function Execution Context, FEC
함수를 호출할 때마다 생성되는 함수 실행 컨텍스트

## Execution Context Stack
실행 컨텍스트가 생성되면 흔히 **콜 스택(Call Stack)** 이라고 불리는 실행 컨텍스트 스택에 쌓이게 된다.

- GEC는 코드를 실행하기 전에 쌓이고 모든 코드를 실행하면 제거된다.
- FEC는 함수를 호출할 때 쌓이고 호출이 끝나면 제거된다.

![[execution-stack.gif]]

## Execution Context Element
실행 컨텍스트는 다음의 구성 요소를 가지고 있다.
- **Lexical Environment**
- **Variable Environment**
- **this binding**

### Lexical Environment
Lexical Environment는 **변수 및 함수 등의 식별자(identifier) 및 외부 참조에 관한 정보를 가지고 있는 컴포넌트** 이다.

Lexical Environment는 다음의 2가지 구성요소를 갖는다.
- **Environment Record**
- **Outer Environment Reference**

#### Environment Record
Environment Record는 식별자에 관한 정보를 가지고 있다.

#### Outer Reference
Outer Environment Reference는 외부 Lexical Environment를 참조하는 포인터이다. 

```js
var x = 10;
 
function foo() {
  var y = 20;
  console.log(x);
}
```

```js
// Lexical Environment
globalEnvironment = {
	environmentRecord = { x: 10 },
	outer: null
}
fooEnvironment = {
	environmentRecord = { y: 20 },
	outer: globalEnvironment
}
```

### Variable Environment
Variable Environment는 Lexical Environment와 동일한 성격을 띠지만 **var로 선언된 변수만 저장한다는 점에서 다르다.**

즉, Lexical Environment는 `var` 로 선언된 변수를 제외하고 나머지(`let` 으로 선언되었거나 함수 선언문)를 저장한다.

> [!tip]
> [Variable Environment vs lexical environment](https://stackoverflow.com/questions/23948198/variable-environment-vs-lexical-environment)

### this binding
this의 바인딩은 실행 컨텍스트가 생성될 때마다 this 객체에 어떻게 바인딩이 되는지를 나타낸 것이다. 

> [!tip]
> ES6부터 this의 바인딩이 LexicalEnvironment 안에 있는 EnvironmentRecord 안에서 일어난다는 사실을 기억해두도록 하자.

- **GEC의 경우**
    - strict mode라면 `undefined` 로 바인딩된다.
    - 아니라면 글로벌 객체로 바인딩된다. (브라우저에선 window, 노드에선 global)
- **FEC의 경우**
    - 해당 함수가 어떻게 호출되었는지에 따라 바인딩된다.

## Phase 
Environment Context는 다음의 2가지 과정을 건치다.

1. **Creation Phase (생성 단계)**
2. **Execution Phase (실행 단계)**

### Creation Phase
Creation Phase는 다시 다음의 3가지 단계로 이루어진다.

1. **Lexical Environment 생성**
2. **Variable Environment 생성**
3. **this 바인딩**

> [!danger]
> 여기서 주의할 점은 생성 단계에 **값이 변수에 매핑되지 않는다는 것**이다.

`var`의 경우 `undefined`로 초기화 되고 `let`, `const`는 아무런 값도 가지지 않는다.






## Reference
[Must Know About Frontend / javascript / execution-context](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/execution-context.md)