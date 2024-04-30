
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
    
첫 배포 이후 운영 과정에서 새로운 요구 사항이 발생하였고 이를 만족하기 위해 기존 기능을 변경하고 새로운 기능을 추가할 필요가 있었다. 금방 끝낼 수 있을거라는 처음의 예상과는 달리 생각 이상의 많은 시간이 소요되었다. 무엇이 문제였을까? 당시 만들었던 버튼 컴포넌트의 제작과정을 살펴보며 문제점을 살펴보자.

> [!info]
> 아래의 링크를 통해 당시 만들었던 Button 컴포넌트 제작 내용을 확인할 수 있다.
> 
> - [feat: button 컴포넌트 구현 #6](https://github.com/effective-tech-interview/effective-tech-interview-client/pull/6)

![[first-buton-design.webp|]]

> 초기 버튼 컴포넌트의 디자인

버튼의 모양에 따라 크게 두 가지 버튼으로 나눠볼 수 있고 사용되는 컬러에 따라 또 버튼을 나눠 생각해볼 수 있다. 버튼을 나누는 명확한 컨테스트가 존재하지 않았기 때문에 디자인된 버튼이 가지는 역할과 책임을 뚜렷하게 정의하기 어려웠고 결국 아래와 같은 버튼 컴포넌트를 만들게 되었다.


```tsx
import { css } from '@emotion/react';
import styled from '@emotion/styled';
import type { ComponentProps, PropsWithChildren } from 'react';

import type { KeyOfColors } from '~/styles/Theme';
import { theme } from '~/styles/Theme';

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

type ButtonStyleProps = Pick<ButtonProps, 'width' | 'height' | 'color' | 'backgroundColor'>;

const StyledButton = styled('button')<ButtonStyleProps>`
  ...
`;
```

버튼 컴포넌트는 다음과 같은 두 가지 문제점을 가지고 있었다.

#### 역할과 책임
현재의 버튼 컴포넌트는


#### 유지 보수성과 확장성



이것을 만족하기 위해 본격적으로 기존 코드를 수정하기 시작하면서 유지 보수에 생각 이상의 많은 비용이 들어간다는 것을 몸소 이해할 수 있었다.


이러한 경험을 통해 유지 보수가 용이한 프로그램을 만들기 위해 노력하게 된 거 같다. 그리고 이를 만족하기 위해서는 각각의 컴포넌트를 역할과 책임에 따라 명확히 나누고 관리하는 것이 중요하다는 것 또한 알게되었다.

그리고 이러한 깨달음들은 결국 SOLID를 만족하는 것이라는 것을 알게되었다.

이러한 깨달음을 바탕으로 다음과 같은 SOLID를 이번 과제에도 적용하기 위해 노력하였다.

> [!info]
> - [프론트엔드와 SOLID 원칙](https://fe-developers.kakaoent.com/2023/230330-frontend-solid/)

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