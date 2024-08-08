Package Manager는 패키지를 다루는 작업을 편리하고 안전하게 수행하기 위해 사용되는 툴

패키지를 다루는 작업
1. 설치
2. 업데이트
3. 수정
4. 삭제

### 패키지

- 패키지는 라이브러리와 비슷한 개념으로, 라이브러리가 코드의 묶음이라면, 패키지는 **코드의 배포**를 위해 사용되는 코드의 묶음이다.j
- 패키지는 일반적으로 라이브러리나 실행 파일(executable)을 포함한다.
- 패키지는 다음 3가지 정보를 가지고 있는 코드의 배포 단위이다.
    1. 컴파일한 소프트웨어의 바이너리(binary)
    2. 환경 설정(configuration)에 관련된 정보
    3. 의존(dependency)에 관련된 정보

### Dependency

- 많은 패키지들은 다른 패키지가 설치되어 있어야만 제대로 동작한다. 이 경우에 기존 패키지를 제대로 동작시키기 위해 필요한 다른 패키지를 **'dependency'** 라고 한다.
- 패키지를 사용하고자 할 때 dependency에 해당하는 패키지들을 전부 설치해야 한다.

#### Dependency Hell

- dependency들을 설치하는 도중, 설치하고 있는 dependency의 dependency를 설치해야 하는 상황이 발생할 수 있다.
- 이런 상황이 끊임없이 이어질 경우 사용자가 수동으로 패키지를 관리하기가 굉장히 어려워진다.
- 이런 경우를 **Dependency Hell**이라고 한다.

👉 각각의 패키지가 자신의 dependency에 대한 정보를 가지게 한다면, 사용하고자 하는 패키지의 dependency를 패키지 매니저를 통해 쉽게 설치할 수 있다.

### 패키지 매니저가 수행하는 일

1. 패키지의 dependency 관리
2. 패키지의 보안 관리 - 신뢰할 수 있음(authenticity), 손상되지 않음(integrity) 보장
3. 여러 패키지를 기능에 따라 그룹으로 묶어 정리
4. 패키지 압축 해제
5. Software repository로부터 패키지를 찾고, 다운로드하고, 설치하고, 업데이트하는 역할

### 패키지 매니저가 동작하는 세 단계
패키지 매니저는 보통 `Resolution`, `Fetch`, `Link` 세 단계로 동작한다.

![[Pasted image 20240808134325.png]]

#### Resolution 

> [!summary] 
> - 라이브러리 버전 고정
> - 라이브러리의 다른 의존성 확인
> - 라이브러리의 다른 의존성 버전 고정

1. 라이브러리 버전 고정
   `package.json`에 명시된 버전 범위에 따라 정확한 버전을 결정한다.
   
   버전 관리에 대한 문서 참조: [유의적 버전 2.0.0](https://semver.org/lang/ko/)
   패키지 매니저는 범위를 만족하는 선에서 가능한 **최신 버전**을 사용하려고 한다.

2. 라이브러리의 다른 의존성 확인
   JavaScript에서는 패키지끼리 의존성을 갖는 상황이 흔하다.
   따라서 **의존성이 또 어떤 의존성을 가지는지 확인**하는 작업이 필요하다.

3. 라이브러리의 다른 의존성 버전 고정
   2에서 확인한 라이브러리의 의존성 버전 또한 고정해야 한다.

> 따라서 JavaScript에서는 의존성의 버전을 범위로 명시하고, 패키지 간에도 의존성을 가지기 때문에, 똑같은 `package.json`에 대해서도 사용하는 의존성 버전이 완전히 달라질 수 있다.
> 
> Resolution 단계는 모든 기기에서 고정된 버전을 사용할 수 있도록 한다.
> 
> 의존성 버전을 전부 고정시키고, 의존성의 의존성을 모두 찾아 버전을 고정시키며, 그 결과물을 `yarn.lock`이나 `package-lock.json`에 저장한다.

#### Fetch

> [!summary] 
> - 결정된 버전의 파일을 다운로드 하는 과정

Resolution의 결과로 결정된 버전을 실제로 다운로드 한다.
`package-lock.json` / `yarn.lock`에 명시된 버전의 패키지를 다운로드 한다.

일반적으로 99%는 npm 레지스트리에서 다 받아온다.

#### Link

> [!summary]
> - Resolution / Fetch 된 라이브러리를 소스 코드에서 사용할 수 있는 환경을 제공하는 과정

npm, pnpm, PnP(Plug'n'Play)의 사례를 각각 살펴보자.

1. npm Linker
   `package.json`에서 명시하는 모든 의존성을 **`node_modules` 디렉토리 밑에다가 하나하나씩 추가한다.**

```shell
my-service/
└─ node_modules/
|  ├─ react/
|  |  
|  └─ @tossteam/tds-mobile/
|     └─ node_modules/
|         └─ @radix-ui/react-dialog
|
└─ src
    └─ index.ts
```

> [!warning] 문제점
> 이러한 방식은 패키지를 찾기 위해서 node_modules를 계속 타고 올라가 파일을 여러 번 읽어야 한다.
> 따라서 다음과 같은 문제가 발생한다.
> 
> 1. import, require 하는 속도가 느려진다.
> 2. 디렉토리의 크기가 너무 커진다
>    
> 이러한 문제를 해결하기 위해 호이스팅(Hoisting)이라는 특이한 방법을 사용하기도 하지마 









### Software repository (= repos)

- 패키지를 저장하고 관리하는 저장소
- 커뮤니티에 기여하는 것을 목적으로 다른 사용자들을 위해 패키지를 등록할 수 있다.
- 성능 문제(load balancing)와 위기상황 대처(fault tolerant)를 위해 여러 개로 분리되어 있으며, 각각의 저장소가 동일한 기능을 수행한다.

### 패키지 매니저 종류

| Language | Package manager   | Software repository |
| -------- | ----------------- | ------------------- |
| Python   | pip               | PyPI                |
| PHP      | Composer          | Packagist           |
| Node.js  | NPM, Yarn         | NPM, Yarn           |
| Java     | Maven, Gradle     | Maven               |
| Ruby     | RubyGems, Bundler | RubyGems, Bundler   |


**Reference**
[패키지 매니저(Package Manager)란?](https://aahc.tistory.com/14)
[npm, yarn, pnplkm 비교해보기](https://yceffort.kr/2022/05/npm-vs-yarn-vs-pnpm)