## The Problems
ES 모듈이 브라우저에서 유효하기 이전에 자바스크립트 자체에 모듈화 기능을 제공하지 않았기 때문에 브라우저에서 실행 가능한 파일로 소스 코드를 연결하는 "bundling"이라는 컨셉에 친해질 수 밖에 없었다.

webpack, Rollup, Parcel과 같은 여러 좋은 번들러 도구가 이미 존재하지만 어플리케이션의 발전에 따라 자바스크립트의 양이 엄청나게 증가하면서 자바스크립트를 이용한 도구에 성능적 병목 현상을 경험하게 되었다. 특히 HMR을 이용한 dev server와 slow feedback은 개발자 경험을 매우 해친다. 

Vite는 이러한 문제를 해결하기 위해 고안되었다.

## Slow Server Start
기존의 번들러는 dev server 실행시 전체 어플리케이션을 빌드한다.

Vite는 **dependencies**와 **source code**라는 두 가지 카테고리로 어플리케이션의 모듈을 나눔으로써 dev server의 시작 시간을 향상시켰다.

**Dependencies**
dependencies는 대부분 평범한 자바스크립트이며 개발 과정에서 잘 변경되지 않는다. 

Vite는 esbuild를 이용하여 dependecies를 pre bundle 한다. 

**Source code**
soure code는 JSX, CSS 와 같은 트랜스파일링이 필요한 파일들을 포함하며 그것들은 매우 자주 변경된다. 

Vite는 native ESM을 이용하여 소스 코드를 제공한다.

Vite는 브라우저에 요청되었을 때 필요하에 소스 코드를 변경하고 그것을 제공한다.

![[Pasted image 20240622121701.png]]

![[Pasted image 20240622121711.png]]

## Slow Updates
bundler 기반의 설정에서 파일을 편집할 때 전체 번들을 다시 빌드하는 것은 매우 비효율적이다.

