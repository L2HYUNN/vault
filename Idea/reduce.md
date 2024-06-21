```js
// 4. _reduce 만들기
var slice = Array.prototype.slice;

function _rest(list, num) {
	return slice.call(list, num || 1);
}

function _each(list, iter) {
	for (var i = 0; i < list.length; i++) {
		iter(list[i]);
	}
	return list;
}

function _reduce(list, iter, memo) {
	if (arguments.length == 2) {
		memo = list[0];
		// list = list.slice(1);
		list = _rest(list);
	}

	_each(list, function(val) {
		memo = iter(memo, val);
	})
	return memo;
}

console.log(
	_reduce([1, 2, 3, 4], function(a, b) { return a + b; }, 0);
) // 10 

// memo를 전달하지 않을 때
console.log(
	_reduce([1, 2, 3, 4], function(a, b) { return a + b; });
) // 10 
```

`reduce`는 축약하는 함수이다. 

> 모든 list에 있는 데이터들을 iter 함수를 통해 축약시켜 새로운 결과를 만든다.


`reduce` 로직에 `slice` 메소드를 사용시 `array`에만 동작하는 `reduce`가 되는 문제가 있다. 

```js
var a = document.querySelectorAll('*');
```

`array-like` 객체에는 `array`의 메서드를 사용할 수 없지만 이것을 가능케하는 방법이 있다.

```js
var a = document.querySelectorAll('*');

var slice = Array.prototype.slice;

slice.call(a, 2);
```