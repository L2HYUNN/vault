## 일급 함수
함수를 값으로 다룰 수 있는 개념

```js
// 변수에 함수를 담을 수 있다.
var f1 = function(a) { return a * a };
```

```js
// 함수를 인자로 받아 사용할 수 있다.
function f3(f) {
	return f();
}

f3(function () { return 10; });
```

> [!tip]
> 즉, 함수형 프로그래밍은 언제 평가해도 상관없는 **순수함수**들을 많이 만들고, 순수 함수들을 값으로 들고 다니면서 **필요한 시점 마다 평가**를 하면서 다양한 로직을 만드는 것

## add_maker

```js
function add_maker(a) {
	return function(b) {
		return a + b;
	}
}

var add10 = add_maker(10);

console.log(add10(20));
```

add_maker가 반환하는 함수는 **클로저**이다.

add_maker가 반환하는 함수는 add_maker의 인자인 a에 대한 컨텍스트를 기억하고 있는 클로저가 된다.

add_maker는 일급함수와 클로저의 개념이 함께 사용된 예제이다. 

또한 이 함수는 동일한 입력에 동일한 결과를 반환하며, 부수효과가 없는 순수함수이다.

```js
function f4(f1, f2, f3) {
	return f3(f1() + f2());
}

f4(
	function() { return 2; },
	function() { return 1; },
	function(a) { return a * a; }
);
```

> 함수형 프로그래밍은 위와 같은 형태를 많이 띈다.

순수 함수로 평가 시점을 다루면 비동기가 일어나는 시점, 동시성이 필요한 시점에서 특정한 함수를 실행해야하는 시점까지 함수를 값으로 다루다가 원하는 시점에 평가를 하거나 반복문을 통해 받아 놓은 함수를 여러번 실행하는 등의 이점을 얻을 수 있다.