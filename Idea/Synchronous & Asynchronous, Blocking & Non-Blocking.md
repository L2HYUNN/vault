> [!info]
> `동기 / 비동기` 와 `블로킹 / 논블로킹` 은 다른 차원의 작업 수행 방식을 설명하는 개념이다. `동기 / 비동기` 는 요청한 작업에 대한 완료 여부를 신경 써서 **작업을 순차적으로 수행할지 아닌지**에 대한 관점이고 `블로킹 / 논블로킹`은 단어 그대로 **현재 작업이 block(차단, 대기) 되느냐 아니냐**에  따라 작업을 수행할 수 있는지에 대한 관점이다.

![[synchronous-asynchronous.png|600]]

대표적으로 자바스크립트의 `setTimeout()` 함수를 일반적으로 비동기 함수라고만 부르지만 동시에 논블로킹 함수이기도 하다. 즉, 우리가 편의상 부르는 자바스크립트 비동기 함수는 사실 비동기 + 논블로킹 함수인 것이다. 따라서 이들을 정확하게 구분하고 이해하는 것이 컴퓨터 아키텍쳐를 이해하는데 있어 중요하다.

## 동기(Synchronous) / 비동기(Asynchronous)
Synchronous는 작업 시간을 함께 맞춰서 실행한다 라는 뜻으로 해석된다. 작업을 맞춰 실행한다는 말은 **요청한 작업에 대해 완료 여부를 따져** 순차대로 처리하는 것을 말한다. Asynchronous는 앞에 A라는 접두사가 붙어 부정하는 형태이다. 그래서 동기와 반대로 **요청한 작업에 대해 완료 여부를 따지지 않기** 때문에 자신의 다음 작업을 그대로 수행하게 된다.

![[sync-async.png|600]]

> 동기는 작업 B가 완료되어야 다음 작업을 수행하고, 비동기는 작업 B의 완료 여부를 따지지 않고 바로 다음 작업을 수행한다

> [!tip]
> 동기(Synchronous)는 하나의 작업이 완료되어야 다음 작업을 진행하는 것이고, 비동기(Asynchronous)는 작업의 완료와 상관 없이 다음 작업을 수행하는 것을 의미한다. 동기(Synchronous)는 요청에 따른 응답을 받아야 다음 요청을 처리하고 비동기(Asynchronous)의 경우 다수의 요청을 받아 먼저 처리하는 순서대로 응답을 처리한다.

### 비동기의 성능 이점
`I/O` 작업과 같은 느린 작업에서, 비동기 방식을 이용하면 `Task`의 기다림 없이 동시에 작업을 처리할 수 있기 때문에 전반적인 시스템 성능의 이점을 얻을 수 있다.

![[sync-async-performance.png|600]]

> [!nfo]
> 비동기에서 사용되는 `동시처리` 개념은 두 개 이상의 작업이 동시에 실행되는 것을 의미한다. 이것은 `멀티 스레드`, `멀티 프로세싱`과 같은 방식을 통해 구현할 수 있다.

### 동기와 비동기는 작업 순서 처리 차이
동기 작업은 요청한 작업에 대한 순서가 지켜지고, 비동기 작업은 순서가 지켜지지 않을 수 있다.
![[sync-task.png|600]]![[async-task.png|600]]
## Blocking / Non - Blocking
블로킹(Blocking)과 논블로킹(Non-Blocking)은 다른 요청 작업을 처리하기 위해 현재 작업을 block(차단, 대기) 하는지 안하는지의 차이를 나타내는 프로세스 실행 방식이다.

> [!tip]
`동기/비동기`가 **전체적인 작업에 대한 순차적인 흐름 유무**라면, `블로킹/논블로킹`은 전체적인 **작업의 흐름을 막는지 안 막는지**로 볼 수 있다.

![[blocking-nonBlocking.png|600]]

### 비동기와 논블로킹 개념 차이
자바스크립트의 `setTimeout` 함수는 비동기, 논블로킹 함수이다. 바라보는 시점에 따라 두 개념을 같게 볼 수 있지만 이것은 엄연히 다른 개념이다.
### 비동기 논블로킹과 콜백 함수
콜백(callback) 함수는 비동기 논블로킹을 구현하는 하나의 기술이지 개념이 아니다. Promise 객체를 이용하거나 이벤트를 이용해서도 비동기 논블로킹을 구현할 수 있다. 

> [!info]
> 콜백은 비동기 논블로킹을 구현하는 하나의 기술일 뿐 개념과 직접적인 관련은 없다.

```js
/* 콜백 함수 방식으로 구현한 비동기 + 논블로킹 서버 요청 작업 */

$.ajax({
  url: 'https://jsonplaceholder.typicode.com/todos/1', 
  type: 'GET', 
  dataType: 'json', 
  success: function(data) { // 요청이 성공하면 호출될 콜백 함수
    console.log(data); 
  },
  error: function(err) { // 요청이 실패하면 호출될 콜백 함수
    throw err;
  }
});

// 요청을 보내는 동시에 다른 작업을 수행할 수 있습니다.
console.log('Hello');
```

```js
/* 프로미스 객체 방식으로 구현한 비동기 + 논블로킹 서버 요청 작업 */

fetch('https://jsonplaceholder.typicode.com/todos/1') 
  .then((response) => {
    return response.json(); 
  })
  .then((data) => {
    console.log(data); 
  })
  .catch((err) => {
    throw err; 
  });

// 요청을 보내는 동시에 다른 작업을 수행할 수 있습니다.
console.log('Hello');
```

```js
/* 이벤트 방식으로 구현한 비동기 + 논블로킹 파일 다운 작업 */

const fileReader = new FileReader(); // FileReader 객체를 생성
const input = document.querySelector('input');
const file = input.files[0]; // input 태그에서 선택된 파일을 가져옵니다

fileReader.addEventListener('load', function() {
	console.log(fileReader.result); // 파일이 load가 완료되면 내용을 출력합니다.
});

fileReader.addEventListener('error', function(err) {
	throw err;
});

// 요청을 보내는 동시에 다른 작업을 수행할 수 있습니다.
console.log('Hello');
```

## 제어권
> [!tip]
> 제어권 개념을 이용하면 `동기/비동기`와 `블로캉/논블로킹`을 보다 명확히 구분할 수 있다.

**제어권**은 함수의 코드나 프로세스의 실행 흐름을 제어할 수 있는 권리를 의미한다.
이 개념은 운영체제(OS)의 커널(kernel)에서 따온 개념으로 I/O 동작에서 설명하는 부분이다.

예를들어 Blocking과 Non-Blocking은 호출된 함수(calle)가 호출한 함수(caller)에게 **제어권을 주느냐 안 주느냐**로 구분할 수 있다. 제어권이 넘어가버리면 해당 스레드는 블로킹되게 된다.

![[blocking-control.png|400]]
A 함수와 B 함수 작업에 대해 A 함수가 B 함수를 **Blocking** 방식으로 호출하는 경우

![[non-blocking-control.png|400]]
A 함수와 B 함수 작업에 대해 A 함수가 B 함수를 **Non-Blocking** 방식으로 호출하는 경우

## Reference
 [https://inpa.tistory.com/entry/👩‍💻-동기비동기-블로킹논블로킹-개념-정리](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC) [Inpa Dev 👨‍💻:티스토리]