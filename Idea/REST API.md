학술적으로 엄밀한 정의는 다음의 논문을 참고하자

> [!note]
>  [로이필딩의 논문](https://www.ics.uci.edu/~taylor/documents/2002-REST-TOIT.pdf)

## REST
REST란 REpresentational State Transfer의 약자로 전반적인 웹 어플리케이션에서 상호작용하는데 사용되는 웹 아키텍쳐 모델이다.

> [!tip]
> 즉, 자원을 주고받는 **웹 상에서의 통신 체계에 있어서 범용적인 스타일을 규정한 아키텍쳐** 라고 할 수 있다.

## API
API란 Application Programming Interface의 약자로 구글 맵 API, 카카오 비전 API 등 **기존에 있는 응용 프로그램을 통해서 데이터를 제공받거나 기능을 사용하고자 할 때 사용하는 인터페이스 및 규격** 을 말한다.

API는 프로그래밍 언어, 운영체제 등에서도 사용되는 범용적인 용어이다. 

> [!summary]
> **REST API**라는 것은 REST 원칙을 적용하여 서비스 API를 설계한 것을 말한다.

## Feature

### Uniform Interface

### Stateless

### Cacheable

### Self-Descriptiveness

### Client-Server Architecture

### Layered System

## Core

### URI는 리소스를 표현해야 한다.

- **리소스 명은 동사가 아닌 명사를 사용해야 한다.**

```
/students/1
```

- **리소스는 Collection과 Document로 표현할 수 있다.**

이 때 Collection은 **복수** 를 사용함을 주의하자.

```
/locations/seoul/schools/3
```

여기서 `locations` 는 Collection을, `seoul` 은 Document를 표현한다.

### 리소스에 대한 행위는 HTTP의 Method로 표현해야 한다.

- **GET은 리소스를 조회한다. (학생 목록 조회)**

```
GET /students
```

- **POST는 리소스를 생성한다. (학생 생성)**

```
POST /students
```

- **PUT은 리소스를 업데이트한다. (1번 학생 정보 업데이트)**

```
PUT /students/1
```

- **DELETE는 리소스를 삭제한다. (1번 학생 삭제)**

```
DELETE /students/1
```

## HTTP Status Code

요청에 대한 응답의 상태코드 또한 명확하게 돌려주는 것이 잘 설계된 REST API이다.

- **2xx** : 성공 관련 (200 Ok, 201 Created)
- **3xx** : 리다이렉션 관련 (304 Not Modified)
- **4xx** : 클라이언트 에러 관련 (400 Bad Request, 401 Unauthorized)
- **5xx** : 서버 에러 관련 (500 Internal Server Error)

## 잘못된 REST 사용

- **GET/POST의 부적합한 사용** : 기존의 조회/생성의 기능이 아닌 다른 방식으로 사용하는 경우이다.
- **자체 표현적이지 않음** : REST의 특징 중 하나인 자체표현성에서 떨어지는 경우로 이해하기 어렵다.
- **HTTP 응답 코드 미사용** : 위에서 정리한 응답에 관한 상태코드를 명확하게 정의하지 않은 경우이다.
- 그 외에 리소스 표현 가이드 및 REST의 특징을 위반한 경우들을 주의하자.

## Reference
[Must Know About Frontend / javascript / REST-API](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/network/rest-api.md)