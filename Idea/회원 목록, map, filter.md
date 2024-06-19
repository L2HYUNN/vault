명령형으로 코드를 짜고 함수형으로 리팩토링을 진행하면서 함수형 프로그래밍에 대해 알아보자

```js
var users = [
  { id: 1, name: 'ID', age: 36 },
  { id: 2, name: 'BJ', age: 32 },
  { id: 3, name: 'JM', age: 32 },
  { id: 4, name: 'PJ', age: 27 },
  { id: 5, name: 'HA', age: 25 },
  { id: 6, name: 'JE', age: 26 },
  { id: 7, name: 'JI', age: 31 },
  { id: 8, name: 'MP', age: 23 }
];

// 1. 명령형 코드
  // 1. 30세 이상인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age >= 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);

  // 2. 30세 이상인 users의 names를 수집한다.
var names = [];
for (var i = 0; i < temp_users.length; i++) {
  names.push(temp_users[i].name);
}
console.log(names);

  // 3. 30세 미만인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age < 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);

  // 4. 30세 미만인 users의 ages를 수집한다.
var ages = [];
for (var i = 0; i < temp_users.length; i++) {
  ages.push(temp_users[i].age);
}
console.log(ages);

// 2. _filter, _map으로 리팩토링
function _filter(list, predi) {
	var new_list = [];
	for (var i = 0; i < list.length; i++) {
		if(predi(list[i])) {
			new_list.push(list[i]);
		}
	}
	return new_list;
}

function _map(list, mapper) {
	var new_list = [];
	for (var i = 0; i < list.length; i++) {
	  new_list.push(mapper(list[i]));
	}
	return new_list;
}

var over_30 = _filter(users, function(user) { return user.age >= 30; });

console.log(over_30);

var names = _map(over_30, function(user) {
	return user.name;
});

console.log(names);

var under_30 = _filter(users, function(user) { return user.age < 30; });

console.log(under_30);

var ages = _map(under_30, function(user) {
	return user.age
});

console.log(ages);

console.log(
	_map(
		_filter(users, function(user) { return user.age >= 30; }),
		function(user) { return user.name; })
	)
);

console.log(
	_map(
		_filter(users, function(user) { return user.age < 30; }),
		function(user) { return user.name; })
	)
);


console.log(_filter([1, 2, 3, 4], function(num) { return num % 2; }));

console.log(_filter([1, 2, 3, 4], function(num) { return !(num % 2); }));
```

filter와 같은 함수를 응용형 함수라고 부른다.

함수가 함수를 받아서 원하는 시점에 해당하는 함수가 알고있는 인자를 적용하는 식으로 프로그래밍 하는 것이 응용형 함수, 응용형 프로그래밍 혹은 적용형 프로그래밍이라고 말한다. 

또한 filter와 같은 함수를 고차 함수라고 부른다. 

고차 함수는 함수를 인자로 받거나 함수를 반환하는 함수를 말한다. 

> 함수형 프로그래밍에서는 대입문을 지양하는 경향이 있다.
> 
> 함수형 프로그래밍은 값을 만들어 놓고 문장을 내려가면서 변형해 나가는 것이 아니라 함수를 통과해나가면서 한 번에 값을 만들어 내는 방식으로 프로그래밍 한다.

> [!tip]
> 함수형 프로그래밍에서는 중복을 제거하거나 어떤 부분을 추상화 할 때 함수를 사용하면 된다.
> 
> 추상화의 단위가 객체나 매서드나 클래스가 아닌 **함수를 추상화의 단위로 이용**해서 프로그래밍 하는 것이 함수형 프로그래밍이다.

반복문에 존재하는 중복을 없애기 위해 `filter`와 `map` 함수를 만들었지만 아직 두 함수에 loop문을 도는 중복이 존재한다.
