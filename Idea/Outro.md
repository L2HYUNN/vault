1. `innerHTML`, `createElement`를 통해 컴포넌트를 만들고 디스플레이한다.
2. `eventListener`를 부착하여 동적으로 동작할 수 있게 만든다. 
	1. 새로 만들어지는 각각의 컴포넌트에 `eventListener`를 부착하는 방법
	2. 부모 컴포넌트 하나에 부착하고 내부적으로 `event.target.matches`를 이용해 원하는 객체에 대한 이벤트를 추려서 사용을 하는 방법
3. `innerHTML`을 사용할 때 생기는 부작용에 유의하자.
	1. `scroll position`이 날라간다.
	2. `input element`에 작성중이던 `state`가 날라간다.
4. `jquery`가 처음에 나와 혁신적이었던 것은 이러한 코드들을 단순하게 바꾸어주었다.

```js
$(".cards").click(() => {})
```

> 그렇다면 `jquery`를 사용하다 `React`, `Vue`와 같은 라이브러리로 전환한 이유는 무엇일까?

