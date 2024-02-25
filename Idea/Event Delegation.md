이벤트 위임(Event Delegation)을 알기 위해서는 이벤트 버블링(Event Bubbling)과 이벤트 캡처링(Event Capturing)의 동작방식을 알아야 한다.

## Event Bubbling
`이벤트 버블링`이란, 하위 엘리먼트에 이벤트가 발생할 때, 그 엘리먼트로 부터 상위요소까지 이벤트가 전파되는 방식을 말한다.

![[event-bubbling.png|500]]

## Event Capturing
`이벤트 캡쳐링`이란, 하위 엘리먼트에 이벤트 핸들러가 있을 때, 상위 엘리먼트로부터 발생한 이벤트가 하위 엘리먼트까지 전파되는 방식을 말한다.

![[event-capturing.png|500]]

## Event Delegation
`이벤트 위임`이란, 상위 엘리먼트에 있는 이벤트 핸들러를 이용해 하위 엘리먼트들을 제어하는 방식을 의미한다.

이벤트 위임 패턴으로 얻는 이점은 다음과 같다.

- 동적으로 엘리먼트를 추가할 때 핸들러를 고려할 필요가 없다.
- 상위 엘리먼트에 하나의 이벤트 핸들러만 추가하면 되기 때문에 코드가 깔끔하다.
- 메모리가 가지는 이벤트 핸들러의 수가 적어져 퍼포먼스 상의 이점이 있다.

## Reference
[# 이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
