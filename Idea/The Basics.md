JavaScript의 모든 변수는 다른 연산을 실행할 때 관측할 수 있는 행동 집합이 있다.

아래의 예시를 통해 이 말의 의미를 이해해보자.

```js
// Accessing the property 'toLowerCase'
// on 'message' and then calling it
message.toLowerCase();

// Calling 'message'
message();
```

우리는 위 코드를 실행하였을 때 어떤 결과가 나올지 신뢰할 수 없다. 각 연산의 결과가 완전히 `message`라는 변수가 무엇인지에 달려있기 때문에 우리는 그 변수를 알기 전까지 결과를 예측할 수 없다.

우리가 각 동작의 결과를 알 수 있는 이유는 그저 우리가 JavaScript 코드를 작성했기 때문에 `message`라는 변수가 무엇인지 알고있기 때문이다. 

만약 `message`의 타입을 제대로 모르는 상태에서 두번째 줄 처럼 함수를 호출 하였고 그 타입이 `string` 이었다면 함수 호출에서 아래와 같은 타입 에러를 보게될 것이다.

> [!error]
> TypeError: message is not a function

**런타임 이전에 이러한 문제를 방지할 수 있다면 분명 좋을 것이다.**

`string`과 `number`와 같은 일부 원시 타입의 경우 `typeof` 연산자를 사용하면 런타임에도 그들의 타입을 알 수 있다. 하지만 `function`과 같은 다른 것들은 런타임에 타입을 체크할 수 있는 방법이 존재하지 않는다. 아래의 예시를 살펴보자

```js
function fn(x) {
	return x.flip();
}
```

우리가 코드를 보았을때 이 함수는 `flip` 함수를 속성으로 가진 `object`를 전달해야만 동작한다는 것을 알 수 있다. 하지만 Javascript의 이면에는 이것을 확인할 수 있는 아무런 정보가 없다. 순수 JavaScript가 특별한 변수를 가진 `fn` 같은 함수가 어떻게 동작하는지 알 수 있는 유일한 방법은 그것을 실행하고 어떤 동작이 일어났는지 살펴보는 방법 뿐이다.

이러한 종류의 동작은 런타임 이전에 코드가 무엇을 하는지 예측하기 어렵게 만든다. 그리고 그것은 즉 **코드를 작성하는 동안 코드의 동작이 무엇인지 알기 어렵다는 것**을 의미한다.

`type`은 이러한 함수에 어떤 값이 전달될 수 있고 충돌할지를 설명하는 개념이다. JavaScript는 `dynamic typing` 만을 제공하기 때문에 코드를 작성하고 실행해봐야만 무슨 타입을 확인할 수 있다.

이것에 대한 대처 방안은 코드 실행에 앞서 동작을 예측할 수 있는 `static type system`을 이용하는 것이다.

```ts
// type-checker
const message = "hello!";

message(); 
// This expression is not callable. Type 'String' has no call signatures.
```

> [!summary]
> JavaScript의 `dynamic typing`은 런타임 이전에 타입을 체크할 수 없기 때문에 프로그램 실행도중 문제가 발생할 수 있다. 따라서 `static typing`을 이용하면 이러한 실수를 방지할 수 있다.

## Static type-checking
코드 실행 이후 에러를 만나게 되었을 때 바로 문제를 해결할 수 있으면 좋겠지만 항상 그럴 수 있는 것은 아니다. 새로운 기능에 대한 충분한 테스트를 하지 못해 잠재적인 에러가 발생할 수 있고 운 좋게 에러를 사전에 알게 되었더라고 이미 많은 코드를 작성하여 수정이 어려울 수 있다. 

이상적으로 우리는 코드를 실행하기에 앞서 이러한 버그를 찾는데 도움을 줄 수 있는 도구가 있다면 좋을 것이다. 그것이 바로 TypeScript의 `static type-checker`가 하는 일이다. `Static types systems`은 변수들의 모양과 무슨 동작을 할지를 설명해준다.

> [!summary]
> `TypeScript`의 `Static type-cheking`을 이용하면 자바스크립트를 실행하기 전에 타입 에러를 방지할 수있다.

## Non-exception Failures
> [!info]
> `Non-exception Failures`란 예외를 이용하지 않고 오류 또는 실패 조건을 처리하는 상황을 말한다. 전통적으로 프로그래밍 언어와 시스템은 예외를 던지고 잡아내는 방식으로 오류를 처리하지만, 모든 오류 상황이 예외를 던질 필요가 있는 것은 아니다. 따라서 성능이 중요하거나 예외 처리가 적합하지 않은 경우에는 다른 방법을 사용할 수 있다.

 [ECMAScript specification](https://tc39.github.io/ecma262/) 는 무언가 예상치 못한 일이 발생했을 때 언어가 어떻게 행동해야 하는지 분명하게 명시하고 있다.

예를들어 명세서는 콜할 수 없는 것을 호출하려고 하였을 때 에러를 던져야만 한다고 말한다. 아마 이것이 "당연한 동작" 처럼 들릴 수 있겠지만 또한 예를들어 `object`에 존재하지 않는 속성에 접근하려 했을때 에러를 발생시키는 것을 상상할 수 있을 것이다. 대신 JavaScript는 이때 우리에게 다른 동작을 주고 `undefined`를 리턴한다.

```js
const user = {
	name: "Daniel",
	age: 26,
};

user.location; // returns undefined
```

```ts
// type checker
const user = {
	name: "Daniel",
	age: 26,
};

user.location;
// Property 'location' does not exist on type '{ name: string; age: number; }'.
```

> [!note]
> TypeScript는 `typos`, `uncalled functions` 혹은 `basic logic error`와 같은 많은 정당한 버그들을 잡아낸다.

> [!summary]
> TypeScript는 `Non-exception Failures`에 대한 실수를 미연에 방지할 수 있게 만든다.
## Types for Tooling
TypeScript는 코드에 실수가 있을때 버그를 잡아낼 수 있다. 하지만 TypeScript는 또한 처음에 그러한 실수를 만들지 않도록 방지할 수 있다.

`type-checker`는 올바른 변수에 속성에대한 정보를 가지고 있어 우리가 사용하기를 원하는 속성에 대한 정보를 제공해준다. 이를 이용하면 `에러 메시지` 그리고 에디터에서 `코드 완성`을 제공받을 수 있다.

TypeScript를 지원하는 에디터는 `quick fixes`을 제공하여 자동으로 에러를 수정하고, 쉽게 코드를 재구성하고 변수의 정의로 이동할 수 있는 혹은 주어진 변수에 대한 모든 참조 찾는 내비게이션 기능을 통해 리팩토링을 할 수 있다.

> [!summary]
> TypeScript는 버그를 잡아낼 뿐 아니라 실수를 미연에 방지하는 `quick fixes`에 대한 다양한 도구를 제공한다.

## `tsc`, the TypeScript compiler

아래의 스크립트를 이용하면 TypeScript `Compiler`를 전역으로 설치할 수 있다.
`npx` 혹은 비슷한 도구를 이용하면 로컬 환경에서 `tsc`를 실행하는 것도 가능하다.

```shell
npm install -g typescript
```

```ts
// Greets the world.
console.log("Hello world!");
```

```shell
tsc hello.ts
```

> [!info]
> 스크립트 실행 이후 `hello.js` 파일을 반환받을 수 있다.

```ts
// This is an industrial-grade general-purpose greeter function:

function greet(person, date) {

console.log(`Hello ${person}, today is ${date}!`);

}

greet("Brendan");
```

```shell
Expected 2 arguments, but got 1.
```

> [!info]
> TypeScript는 타입 에러가 발생할 경우 위와 같이 문제를 찾아준다.

## Emitting with Errors
`type-checking` 코드는 실행할 수 있는 프로그램의 종류를 제한한다. 그렇기에 `type-checker`가 수용할 수 있는 종류를 찾는 것에 대한 트레이드 오프가 있다. 대부분의 경우 괜찮지만 이러한 트레이드오프가 방해가되는 경우가 있다.

예를들어 JavaScript 코드를 TypeScript로 마이그레이션하고 `type-checking` 에러가 발생한다고 상상해보자. 결국 `type-checker`를 위해 코드를 정리하지만 JavaScript 코드는 이미 동작하고 있다. 왜 TypeScript로 변환하면 실행이 중단되는걸까?

그렇기에 TypeScript는 그러한 방해를 하지 않는다. 물론 시간이 지남에 따라 실수에 대해 조금 더 방어적이고 TypeScript가 조금 더 엄격하게 동작하는 것을 원할지도 모른다.  


> [!info] No Emit On Error - `noEmitOnError`
> 어떤 에러가 발견되었을 때, JavaScript 소스 코드나 소스 맵 혹은 선언 같은 컴파일 결과 파일을 내보내지 않는다.
> 
> 이것의 기본값은 `false` 이며, 이것은 모든 에러가 해결되었음을 확인하기 전에 또 다른 환경에 있는 코드 변화에 대한 결과를 볼 수 있는 `watch-like` 환경에서 TypeScript와 함께 더 쉽게 일하게 만들어 준다.






