```js
// 3. 커링
  // 1. _curry, _curryr
function _curry(fn) {
	return function(a, b) {
		if (arguments.length == 2) {
			return fn(a, b)
		};
		
		return function(b) {
			return fn(a, b);
		}
	}
}

// 오른쪽 인자부터 적용하는 cuury right
function _curryr(fn) {
	return function(a, b) {
		if (arguments.length == 2) {
			return fn(a, b)
		};
		
		return function(b) {
			return fn(b, a);
		}
	}
}

var add = _curry(function(a, b) {
  return a + b;
});

var add10 = add(10);
var add5 = add(5);
console.log( add10(5) );
console.log( add(5)(3) );
console.log( add5(3) );
console.log( add(10)(3) );
console.log( add(1, 2) );


var sub = _curryr(function(a, b) {
  return a - b;
});

console.log( sub(10, 5) );

var sub10 = sub(10);
console.log( sub10(5) );
```

커링은 함수와 인자를 다루는 기법

함수에 인자를 하나씩 적용해나가다가 필요한 인자가 모두 채워지면 함수 본채를 실행하는 기법

자바스크립트에서는 커링을 지원하지 않지만 커링을 직접 구현할 수 있다. 

> 결국 본체인 함수를 값으로 들고 있다가 원하는 시점까지 미뤄뒀다가 최종적으로 평가하는 기법이다.


```js
// 3. 커링
  // 1. _curry, _curryr
  // 2. _get 만들어 좀 더 간단하게 하기
function _get(obj, key) {
	return obj == null ? undefined : obj[key];
}

// 오른쪽 인자부터 적용하는 cuury right
function _curryr(fn) {
	return function(a, b) {
		if (arguments.length == 2) {
			return fn(a, b)
		};
		
		return function(b) {
			return fn(b, a);
		}
	}
}

var _get = _curryr(function(obj, key) {
	return obj == null ? undefined : obj[key];
});

var user1 = users[0];
console.log(user1.name);
console.log(_get(user1, 'name'));
console.log(_get('name')(user1));

// name을 꺼내는 함수가 된다. 
var get_name = _get('name');

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    function(user) { return user.name; }));

console.log(
  _map(
    _filter(users, function(user) { return user.age < 30; }),
    function(user) { return user.age; }));

console.log(
  _map(
    _filter(users, function(user) { return user.age >= 30; }),
    _get('name')));

console.log(
  _map(
    _filter(users, function(user) { return user.age < 30; }),
    _get('age')));
```

`get`을 이용해서 `object`를 안전하게 참조한다.