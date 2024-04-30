
![[29cm-logo.webp]]

> [!info]
> 본 글은 3월에 진행했던 **29CM Frontend Eginner** 전형 과제에 대한 회고입니다. 
> 모든 과제 내용은 **외부 유출이 금지**되어 있기 때문에 공개할 수 없는 점 양해 부탁드립니다.
> 
> 본 회고는 1부, 2부로 나뉘어 작성되었습니다.
> 
> - 1부 - **기술 선정**
> - 2부 - **고민과 후기**

1부에서는 서류 지원과 합격 그리고 과제에 사용할 기술 선정에 대해서 이야기했습니다. 

이번 2부에서는 과제를 진행하면서 고민한 점과 종합적인 후기에 대해서 이야기할 예정입니다.

## 고민
기술 선정 이후 했던 가장 큰 고민은 **"어떻게 프로그램을 잘 만들 수 있을까?"** 였다.

합격을 위해 과제에서 요구하는 바를 모두 만족하는 프로그램을 만들어야 한다는 것에는 이견이 없을 것이다. 하지만 단순히 이러한 프로그램을 만드는 것만으로는 합격이 어려울 수 있다고 생각했다. 많은 사람들이 가고 싶어하는 기업인 만큼 뛰어난 사람들이 해당 전형에 지원했을 것이기 때문이다. 따라서 합격을 위해서는 요구사항을 만족할 뿐 아니라 잘 만들어진 프로그램을 제출할 필요가 있다고 생각했다. 

> 그렇다면 잘 만들어진 프로그램이란 무엇을 의미하는 걸까?

잘 만들어진 프로그램(좋은 프로그램)을 정의하는 다양한 요소가 있겠지만 개인적으로 생각하는 좋은 프로그램이란 바로 **유지 보수와 확장이 용이한 프로그램**을 의미한다. 그동안 다양한 서비스를 만들어 오며 배운 것이 하나 있다면 바로 상황에 따라 **프로그램은 계속 변경된다**는 것이다. 제작 중에 요구사항이 변경되거나 완성 이후에 새로운 기능이 추가되는 등 프로그램은 항상 상황에 따른 변경 사항을 맞이하게 된다. 이때 잘 만들어진 프로그램(좋은 프로그램)이라면 큰 어려움 없이 새로운 기능을 추가하고 변경할 수 있을 것이다.

이러한 생각을 바탕으로 유지 보수와 확장에 용이한 프로그램을 만들기 위해 아래와 같이 고민하였다.
 
### SOLID
[마지막 프로젝트](https://github.com/effective-tech-interview/effective-tech-interview-client)를 통해 잠시나마 실제로 서비스를 운영해 볼 수 있었다.
    
첫 배포 이후 운영 과정에서 새로운 요구 사항이 발생하였고 이를 만족하기 위해 기존 기능을 변경하고 새로운 기능을 추가할 필요가 있었다. 금방 끝낼 수 있을거라는 처음의 예상과는 달리 생각 이상의 많은 시간이 소요되었다. 무엇이 문제였을까? 당시 만들었던 버튼 컴포넌트의 제작과정을 잠시 살펴보며 문제점을 알아보자.

> [!info]
> 아래의 링크를 통해 당시 만들었던 Button 컴포넌트의 제작 내용을 확인할 수 있다.
> 
> - [feat: button 컴포넌트 구현 #6](https://github.com/effective-tech-interview/effective-tech-interview-client/pull/6)
> - [refactor: button 컴포넌트 리팩토링 #11](https://github.com/effective-tech-interview/effective-tech-interview-client/pull/11)
> - [feat: question button 구현 #64](https://github.com/effective-tech-interview/effective-tech-interview-client/pull/64)

![[first-buton-design.webp|]]

> 초기 버튼 컴포넌트의 디자인

초기 버튼 컴포넌트는 스타일에 따른 구분만 존재할 뿐 역할과 책임을 가지는 뚜렷한 구분이 존재하지 않았다. 

따라서 처음에는 아래와 같이 `props`를 통해 디자인을 적용할 수 있는 버튼 컴포넌트를 만들게 되었다.

```tsx
// Button.tsx
interface ButtonProps extends ComponentProps<'button'> {
  width?: number;
  height?: number;
  color?: KeyOfColors;
  backgroundColor?: KeyOfColors;
}

const Button = ({
  width,
  height,
  color,
  backgroundColor,
  children,
  ...rest
}: PropsWithChildren<ButtonProps>) => {
  return (
    <StyledButton
      width={width}
      height={height}
      color={color}
      backgroundColor={backgroundColor}
      {...rest}
    >
      {children}
    </StyledButton>
  );
};

export default Button;
```

> [src/components/common/Button/Button.tsx](https://github.com/effective-tech-interview/effective-tech-interview-client/blob/0aa0558b02ebf78ef2ba125423bed55303e71ad1/src/components/common/Button/Button.tsx)

이와 같이 만든 버튼 컴포넌트는 다음과 같은 **문제점**을 가진다.

현재 버튼 컴포넌트에는 프로젝트의 디자인 시스템이 적용되어 있다. 버튼 컴포넌트는 **도메인**이 적용된 컴포넌트이다. 하지만 도메인에 따른 **역할과 책임**은 배제되어있다. 버튼 컴포넌트의 역햘과 책임은 컴포넌트 자체가 가지는 것이 아닌 컴포넌트를 사용하는 사용자에 의해 결정된다. 

따라서 버튼 컴포넌의 역할과 책임을 이해하기 위해서는 항상 주변 컨텍스트를 이해해야만한다. 이것은 코드의 가독성을 해치고 생산성을 떨어트린다. 

따라서 이러한 문제를 해결하기 위해서 버튼 컴포넌트가 역할과 책임을 가질 수 있게 해야한다.

> [!info]
> 버튼 컴포넌트 개선에 대한 상세한 내용은 추후에 따로 작성할 예정이다.

위와 같이 버튼 컴포넌트 개선 과정을 겪으며 컴포넌트의 **역할과 책임**에 대해서 더욱 깊게 고민하기 시작했다. 그리고 곧 이러한 고민이 `SRP(Single Responsibility Principle)`와 닮아있다는 것을 알게 되었다. SRP에 대해 공부하기 시작하면서 자연스럽게 프론트엔드에서의 `SOLID`에 관심이 가게 되었다.

아래의 글들을 통해 프론트엔드에서의 `SOLID`에 대해 이해할 수 있었다.

> [!tip]
> - [프론트엔드와 SOLID 원칙](https://fe-developers.kakaoent.com/2023/230330-frontend-solid/)
> - [[번역] 그림으로 보는 SOLID 원칙](https://blog.siner.io/2020/06/18/solid-principles/)
> - [프론트엔드에 SOLID 적용하기](https://kooku0.github.io/blog/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%EC%97%90-solid-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/)

과제를 진행하며 모든 원칙을 만족하는 코드를 만들 수는 없었지만 적어도 `SRP(Single Responsibility Principle)`를 충실히 따르기 위해 노력했다. 아래는 과제를 진행하며 사용했던 프로젝트의 폴더 구성이다. 

> 과제의 내용이 노출될 수 있는 부분은 **domain**으로 표현하였습니다.

```zsh
📦src  
 ┣ 📂apis  
 ┣ 📂app  
 ┣ 📂components    
 ┃ ┣ 📂domain01  
 ┃ ┣ 📂domain02  
 ┃ ┣ 📂domain03
 ┃ ┣ 📂provider  
 ┃ ┗ 📂ui  
 ┣ 📂hooks  
 ┣ 📂mocks  
 ┣ 📂queries  
 ┣ 📂services   
 ┣ 📂stores  
 ┣ 📂stories  
 ┣ 📂styles  
 ┣ 📂types  
 ┗ 📂utils  
```

각각의 폴더는 다음과 같은 역할과 책임을 가진다.

- **apis**: Axios를 이용한 데이터 패칭과 관련된 파일을 모아 놓은 디렉토리
- **app**: 어플리케이션의 page를 모아 놓은 디렉토리 Next의 App Router를 사용시 page를 표현하기 위해 app 디렉토리를 사용해야만 한다. 
- **components**: 어플리케이션에서 사용하는 모든 컴포넌트를 모아 놓은 디렉토리. 도메인에 구애 받지 않는 UI 컴포넌트는 UI 폴더에, 이것을 이용하여 도메인과 관련된 컴포넌트는 따로 도메인에 맞는 폴더에 모아 놓는다.
- **hooks**: 어플리케이션의 비지니스 로직을 hook으로 만들어 커스텀 hook을 모아 놓은 디렉토리
- **mocks**: MSW를 이용한 mock 서버와 관련된 파일을 모아 놓은 디렉토리
- **queries**: 서버 상태 관리를 위한 React-Query와 관련된 파일을 모아 놓은 디렉토리
- **services**: 어플리케이션의 비지니스 로직 중 hook이 아닌 파일을 모아 놓은 디렉토리
- **stores**: 전역 상태 관리를 위한 Zustand와 관련된 파일을 모아 놓은 디렉토리
- **stories**: Storybook을 이용한 스토리를 모아 놓은 디렉토리 
- **styles**: 스타일과 관련된 파일을 모아 놓은 디렉토리
- **types**: 전역 혹은 중복으로 사용되는 타입을 모아 놓은 디렉토리
- **utils**: 유틸, 핼퍼 함수를 모아 놓은 디렉토리

### Layer Architecture

> [!info]
> - [프론트엔드 상태관리 실전 편 with React Query & Zustand [#우아콘2023]](https://youtu.be/nkXIpGjVxWU?si=Imt-rjOH4FVZLJHA)

### CDD (Component-Driven Development)

![[cdd-gif.gif]]

가장 먼저 떠올린 방법은 바로 [CDD(Component-Driven Development)](https://www.chromatic.com/blog/component-driven-development/)를 적용하는 것이었다. 이전 프로젝트 부터 컴포넌트를 바텀-업 방식으로 개발하고 있었다. 이러한 바텀 업 개발 방식은 점진적으로 결합(조립)하는 CDD의 성향과 비슷했기 때문에 큰 어려움 없이 개발을 진행할 수 있으며 전체 페이지가 아닌 하나의 컴포넌트에 집중하여 개발을 진행하기 때문에 각 컴포넌트의 역할과 책임에 대해서만 고민할 수 있게 되었다. 

UI Test에 대한 이야기

코드 스닛펫을 적용한 스토리북 작성

실제로 작성한 UI Test 


### TDD (Test-Driven Development)











역할과 책임
선언형 프로그래밍 / 함수형 프로그래밍
프론트엔드 SOLID




### CDD / TDD 
먼저 가장 먼저 떠올린 것은 CDD와 TDD 그리고 그 중 CDD 였습니다. CDD를 이용하면 자연스럽게 바텀-업 방식으로 컴포넌트와 페이지를 구성하게 되며 그 과정을 Storybook을 이용하여 눈으로 확인할 수 있기 때문에 좋은 방법이라고 생각했습니다.

CDD를 적용하다 보면 바텀 업 방식으로 작은 컴포넌트부터 설계하기 때문에 하나의 컴포넌트가 가질 책임과 역할에 대해서 보다 깊게 생각할 수 있게 만든다는 것이 큰 장점이었던 것 같습니다. 완전히 일치하지는 않겠지만 Frontend에서 또한 SOLID를 따르는 것이 보다 더 좋은 컴포넌트를 만드는 방법이라고 생각했기 때문에 ~ 

### 


## 기술적 도전?
- Hydration Provider
- Layer Architecture
- SOLID
- CDD / TDD
- MSW
- axios service
- code snippets
- domain


## 문제 해결

## 아쉬웠던 부분


기존에 사용하지 않았던 많은 기술들을 단시간에 학습하고 적용하려다보니 2일차가 되어서야 본격적으로 과제를 진행할 수 있었다. 또한 Next 14를 사용하면서 많은 버그들에 부딪혔고 이를 해결하는데 많은 시간을 소비해야만 했다. 