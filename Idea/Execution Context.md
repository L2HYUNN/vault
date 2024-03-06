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

### Lexical Environment

### Va