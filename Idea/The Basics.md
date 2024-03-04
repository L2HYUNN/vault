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
 [ECMAScript specification](https://tc39.github.io/ecma262/) 는 무언가 예상치 못한 일이 발생했을 때 언어가 어떻게 행동해야 하는지 분명하게 명시하고 있다.

예를들어 명세서는 콜할 수 없는 것을 호출하려고 하였을 때 에러를 던져야만 한다고 말한다. 아마 이것이 "이상한 동작" 처럼 들릴 수 있겠지만 또한 예를들어 `object`에 존재하지 않는 속성에 접근하려 했을때 에러를 발생시키는 것을 상상할 수 있을 것이다. JavaScript는 이때 우리에게 다른 동작을 주고 `undefined`를 리턴한다.

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
