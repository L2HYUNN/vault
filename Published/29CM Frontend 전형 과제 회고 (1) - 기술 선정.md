---
title: 29CM Frontend 전형 과제 회고 (1) - 기술 선정
date: 2024-04-01 19:40 +0900
categories: "[post, assignment]"
tags: 
image:
---

![[29cm-logo.webp]]

> [!info]
> 본 글은 3월에 진행했던 **29CM Frontend Eginner** 전형 과제에 대한 회고입니다. 
> 모든 과제 내용은 **외부 유출이 금지**되어 있기 때문에 공개할 수 없는 점 양해 부탁드립니다.
> 
> 본 회고는 1부, 2부로 나뉘어 작성되었습니다.
> 
> - 1부 - **기술 선정**
> - 2부 - **고민과 후기**

본격적으로 취업을 준비 하기 시작하면서 취업 공고나 정보를 얻을 수 있는 원티드, 프로그래머스, 로켓펀치, 카카오톡 오픈채팅방 등을 확인하는 것이 하루 일과가 되었다.

당시 이미 여러 기업에 지원을 했던 상태였고 운좋게 면접까지 이어진 곳 또한 있었다. 하지만 아쉽게도 최종 합격까지는 이어지지 못했고 그 과정속에서 얻은 경험을 바탕으로 부족한 부분을 채워가고 있었다.

이렇게 직접 채용 과정을 경험하면서 혼자 준비할 때는 몰랐던 단점들을 발견할 수 있었다. 떨어지는 것은 분명 뼈 아픈 경험이었지만 이를 통해 몰랐던 단점들을 보완할 수 있는 기회를 얻었기 때문에 좌절 보다는 오히려 기회를 얻었다고 생각했다.

이러한 상황에서 우연히 **29CM Frontend Enginner** 공고를 보게되었다. 29CM는 개인적으로 꼭 가고 싶다고 생각한 기업이었기 때문에 설레는 마음으로 채용 공고를 확인해보았다.

![[29cm-frontend-condition.webp|600]]

프론트엔드 개발 경력이 2년 이상이라는 부분에서 신입은 지원할 수 없다고.. 생각했지만 아래의 한 줄이 있어 29CM에 지원을 결심하게 되었다.

> **"29CM는 경력보다 역량을 중요하게 생각하고 있습니다. 경력기간 요건에 부합하지 않더라도 아래 자격요건에 부합하시는 경우 지원 가능합니다."**

## 서류 합격
서류 지원을 마치고 얼마나 시간이 지났을까 지원에 대한 기억이 희미해질 즈음 서류 전형에 대한 결과를 받아볼 수 있었다.

![[29cm-document-passed.webp|600]]

사실 자격 요건에 경력 2년 이상이라는 부분이 있었기 때문에 지원을 하면서도 합격을 기대하기는 어렵다고 생각했다. 서류에 대한 기준이 그만큼 높다고 생각했기 때문이다. 

그렇기에 크게 기대를 하지 않고 있었는데 서류 전형을 합격하게 되다니.. 어안이 벙벙했다. 운이 좋았다고 생각했고 이렇게 기회가 주어진 만큼 할 수 있는 모든 노력을 다해 과제를 진행해야겠다고 생각했다.

> [!tip]
> 혹시나 서류 지원에 사용했던 이력서가 궁금하신 분들을 위해 부족하지만 지원에 사용했던 이력서 및 포트폴리오와 이력서 작성 방법에 대해 작성했던 글을 남긴다.
> - [신입 프론트엔드 개발자 이력서 작성하기](https://l2hyunn.github.io/posts/frontend-developer-resume/)
> - [이동현 이력서](https://team-project22.notion.site/Developer-699a7e05c9f3414088517e3ab3220618)
> - [이동현 포트폴리오](https://team-project22.notion.site/7190b4b78ce540ff97fea14e58aaa3c3?v=fd78ab97bc8345f7a001422cf111a9cf)

## 과제 전형

### 기술 선정
과제를 진행하기 전 처음에 가장 고민했던 것은 **"어떤 기술을 사용해야 하는가?"** 였다. 아래의 사진은 채용 공고에 나와 있는 29CM Frontend 팀이 사용하고 있는 주요 기술 스택 및 도구이다.

![[29cm-frontend-team-stack.webp|600]]

기존에 사용해왔던 기술들을 이용한다면 보다 쉽게 과제를 진행할 수 있겠지만, 가능하면 29CM Frontend 팀에서 사용하고 있는 기술들을 이용하여 과제를 진행하는 것이 좋다고 생각했다. 또한 다음과 같은 이유에서 아래와 같은 기술들을 사용하기로 결심했다.

#### Next 14
작년 6월 Next 13(**Page Router**)을 이용하여 진행했던 [기술 면접 대비 플랫폼](https://github.com/effective-tech-interview/effective-tech-interview-client)을 마지막으로 새로운 프로젝트를 진행하지 않았다. 새로운 프로젝트 보다는 자료구조, 알고리즘이나 자바스크립트 공부에 집중 하였고 그러다보니 새로운 버전의 Next가 나왔지만 아직 제대로 사용해본 적이 없던 상황이었다. 

![[next-app-router.webp|600]]

Next 13에서 기존의 Page Router가 [**App Router**](https://nextjs.org/docs/app)로 변경 되면서 많은 변화가 있었기 때문에 언제까지 Next의 최신 버전을 사용해보지 않을 수는 없다고 생각했다. 그렇다보니 이번에 새로운 개인 프로젝트를 기획 하면서 Next 14를 사용하려고 계획하고 있던 상황이었다.

이러한 상황 속에서 29CM Frontend 팀이 Next 14를 사용하고 있다는 것을 채용공고를 통해 알게 되었고 과제 전형에서 Next 14의 사용을 고려해보게 되었다.

Next를 이용하면 **직관적인 라우팅 시스템, 다양한 랜더링 전략, 캐싱, 이미지와 폰트 최적화** 등 고성능의 웹 어플리케이션을 위한 다양한 기능들을 사용할 수 있기 때문에 서비스의 성능을 생각한다면 좋은 선택일 수 있지만, 서버를 직접 관리하는 것과 새로운 App Router를 학습하고 사용하는 것에 대한 고민이 있었다. 

App Router는 기본적으로 [**서버 컴포넌트**](https://www.patterns.dev/react/react-server-components/)를 사용하기 때문에 이에 대한 충분한 이해가 필요했고 이에 따른 App Router의 전체적인 기능(공유되는 레이아웃, 중첩 라우팅, 로딩 상태, 에러 핸들링등)을 이해할 필요가 있었다. 하지만 그럼에도 서버 컴포넌트가 [다음과 같은 이점](https://nextjs.org/docs/app/building-your-application/rendering/server-components)**(Zero Bundle Size, Automatic Code Splitting, Progressive Rendering)** 을 가지기 때문에 더 나은 성능의 어플리케이션을 만들기 위해 Next 14의 사용을 결정하게 되었다.

> [!info]
> 서버 컴포넌트에 대한 자세한 설명은 [다음의 글](https://nextjs.org/docs/app/building-your-application/rendering/server-components)을 참고하자.
 
#### React-Query 
Next 14에서 서버 컴포넌트를 도입하면서 이를 통해 서버에서 직접 데이터를 요청할 수 있게 되었다. 또한 Next는 기존의 fetch Web API를 확장하여 캐싱(caching)과 재검증(revalidating)이 가능하게 만들었다. 자세한 내용은 아래의 공식 문서를 통해 찾아볼 수 있다.

> [!info]
> - [Data Fetching, Caching and Revalidating](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating)

![[react-query-v5.webp|600]]

이러한 상황에서 **"클라이언트 서버 상태 관리를 위해 Next에 React Query를 사용해야 하는가?"** 에 대한 의문이 생겼고 이러한 의문을 풀기 위해 다양한 자료를 찾아보았다. 

찾아본 자료들에 따르면 Next 14에서 [Server Action](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)이 안정화 되면서 서버에서 모든 데이터를 다룰 수 있기 때문에 React Query의 사용이 불필요하다고 이야기하는 것 같았지만 [특별한 경우](https://www.reddit.com/r/nextjs/comments/19d0sar/comment/kj2pdiq/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) (인피니티 스크롤, 폴링) 등을 구현할 때 도움이 될 수 있다고 이야기하는 것 같았다.

> [!info]
> - [Nextjs 14 Server actions vs React query](https://www.reddit.com/r/nextjs/comments/19d0sar/nextjs_14_server_actions_vs_react_query/)
> - [You Might Not Need React Query](https://tkdodo.eu/blog/you-might-not-need-react-query)
> - [Why I don't use React-Query and tRPC anymore](https://www.youtube.com/watch?v=51pf_nCJpwg&t=6s)

처음에는 Next에서 이미 데이터 패칭을 위한 충분한 기능들을 제공하고 있기 때문에 React Query를 사용하지 않으려 했다. 하지만 현재 Next의 App Router를 처음 사용한다는 상황을 고려해보았을 때 기존에 이용해왔던 React Query를 이용하여 서버 상태를 관리하는 것이 현재 상황에 맞는 기술 선택이라고 생각했다. 또한 결정적으로 29CM Frontend 팀에서 React Query를 사용하고 있었기 때문에 React Query를 사용하기로 결정했다.

#### Zustand
이전에 프로젝트를 진행할 때 클라이언트 상태 관리를 위해 주로 [recoil](https://recoiljs.org/ko/)을 사용하고 있었다. 처음 배웠던 상태 관리 라이브러리이기도 했고 가볍고 쉽게 사용할 수 있기 때문에 주력으로 사용했던 것 같다. 

이와 같은 이유로 라이브러리를 사용하고 있다보니 한 번즈음은 클라이언트 상태 관리 라이브러리들을 비교하고 특징과 장단점을 알고 있을 필요성을 느끼게 되었다. 그렇게 해야 이후 다른 프로젝트를 시작하더라도 상황에 맞는 적절한 라이브러리를 선택할 수 있을 거라는 생각 때문이다.

아래의 글들을 통해 각각의 라이브러리들의 특징과 장단점을 쉽게 정리할 수 있었다.

> [!info]
> - [리액트 상태 관리 라이브러리, 어떤 것을 써야 할까?](https://yozm.wishket.com/magazine/detail/2233/)
> - [상태관리 라이브러리 비교: Redux vs Recoil vs Zustand vs Jotai](https://velog.io/@iberis/%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EB%B9%84%EA%B5%90-Redux-vs-Recoil-vs-Zustand-vs-Jotai)

그렇다면 과제를 위해서는 어떤 라이브러리를 써야할까? 

![[zustand-logo.webp|600]]

많은 고민 끝에 아래와 같은 이유로 [zustand](https://zustand-demo.pmnd.rs/)를 사용하기로 결정하였다. 

##### 1. 최근 가장 많이 사용되고 있는 상태 관리 라이브러리

![[zustand-graph.webp|600]]

[npm trends](https://npmtrends.com/jotai-vs-mobx-vs-recoil-vs-zustand)를 살펴보면 zustand의 다운로드 수가 타 라이브러리들에 비해 높은 것을 확인할 수 있다.

라이브러리를 결정하는데 있어 중요한 사항 중 하나는 **얼마나 많은 사람들이 해당 라이브러리를 사용하는가** 이다. 참고할 자료가 필요하거나 이슈가 발생했을 때 대응할 수 있는 많은 래퍼런스나 커뮤니티가 존재하기 때문이다.

> [!Caution]
> [Redux](https://redux.js.org/)는 스토어 구성을 위한 많은 보일러플레이트 필요 등의 이유로 비교군에서 제외하였다.

##### 2. 작은 패키지 사이즈

![[zustand-stats.webp|600]]

라이브러리의 크기는 곧 번들 사이즈에 영향을 주기 때문에 가능한 작은 사이즈의 패키지를 사용하는 것이 좋다.

위의 사진에서 볼 수 있듯 타 라이브러리들에 비해 압도적으로 작은 패키지 사이즈를 가진 것을 알 수 있다.

##### 3. 간단함
아래와 같이 create를 통해 스토어를 생성하고 이를 가져와 사용하는 것이 전부이기 때문에 낮은 러닝 커브로 간단하게 사용할 수 있다. 

```js
import { create } from 'zustand'

const useStore = create((set) => ({
  count: 1,
  inc: () => set((state) => ({ count: state.count + 1 })),
}))

function Counter() {
  const { count, inc } = useStore()
  return (
    <div>
      <span>{count}</span>
      <button onClick={inc}>one up</button>
    </div>
  )
}
```

##### 4. 중앙화 / 액션 기반 상태 관리

> [!info]
> zustand의 이러한 특징을 알아보기 위해 recoil과 비교를 해보았다. 

recoil의 경우 react와 유사한 동작 방식을 가지기 때문에 컴포넌트 안에서 action을 선언하게된다.

```js
// Atom Store
import { atom } from 'recoil';

export const counterState = atom({
	key: 'counter',
	default: 0
});
```

```jsx
// Use Recoil
import { useRecoilState } from 'recoil';
import { counterState } from '..';

const Counter = () => {
	const [count, setCount] = useRecoilState(counterState);
	// Action
	const handleIncrement = () => {
		setCount(count + 1);
	}
	// Action
	const handleDecrement = () => {
		setCount(count - 1);
	}

	return (
		<div>
			<button onClick={handleDecrement}>-</button>
			{count}
			<button onClick={handleIncrement}>+</button>
		</div>
	)
};
```

보통 atom은 컴포넌트 외부에서 별도의 파일로 관리 되기 때문에 이렇게 store와 action이 분리되는 것은 구조가 복잡해짐에 따라 상태 추적을 어렵게 만들 수 있다. 이러한 문제를 해결하기 위해서는 다음과 같이 커스텀 훅을 만들어 사용하는 방법을 이용할 수 있다.

```js
// Use Custom Hook
import { useRecoilState } from 'recoil';
import { counterState } from '..';

export const useCounter = () => {
	const [count, setCount] = useRecoilState(counterState);
	
	const handleIncrement = () => {
		setCount(count + 1);
	}
	
	const handleDecrement = () => {
		setCount(count - 1);
	}

	return {
		count,
		handleIncrement,
		handleDecrement,
	}
}
```

하지만 zustand를 이용하면 다음과 같이 store 안에서 action을 정의할 수 있다.

```js
// Zustand Store
import create from 'zustand';

const useCounterStore = create((set) => ({
	count: 0,
	setIncrement: () => set(state => ({ state.count + 1})),
	setDecrement: () => set(state => ({ state.count - 1})),
}));
```

```jsx
// Use Zustand
import { useCounterStore } from '..';

const Counter = () => {
	const { count, setIncrement, setDecrement } = useCounterStore();

	return (
		<div>
			<button onClick={setDecrement}>-</button>
			{count}
			<button onClick={setIncrement}>+</button>
		</div>
	)
};
```

따라서 이와 같은 이유에서 클라이언트 상태 관리 라이브러리로 zustand를 결정하게 되었다.

> [!info]
> 참고: [[상태관리] 내가 Zustand를 선택한 이유 (over the Recoil)](https://all-dev-kang.tistory.com/entry/%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-%EB%82%B4%EA%B0%80-Zustand%EB%A5%BC-%EC%84%A0%ED%83%9D%ED%95%9C-%EC%9D%B4%EC%9C%A0-over-the-Recoil)

#### MSW
프론트엔드 개발 과정에서 겪는 불편함 중 하나는 API 개발이 완료되기 전까지 데이터 연동을 위해 기다려야 하는 상황이 발생한다는 것이다. 이전에는 이러한 문제를 해결하기 위해 기존에는 단계별로 다음과 같은 방법들을 이용했다. 

##### 1. 직접 Mocking 하기
처음에는 필요한 데이터를 직접 어플리케이션 내부에 Mocking 하여 사용하는 방법을 이용했다. JS 파일을 생성하고 필요한 데이터를 구성하면 되기 때문에 쉽고 빠르게 적용할 수 있는 방법이었다. 

하지만 API 개발이 완료되면 Mocking으로 사용하던 부분을 수정해야하고 HTTP 메소드와 네트워크 응답 상태에 따른 개발을 하기 어렵다는 단점이 존재했다.

##### 2. Mock 서버 이용하기
다음에는 이러한 문제를 해결하기 위해 [json-server](https://github.com/typicode/json-server/tree/v0)라는 라이브러리를 찾아 직접 Mock 서버를 구성하였다. 이 방법을 이용하면 웹 어플리케이션의 서비스 로직을 수정하지 않아도 된다는 장점이 있지만, 로컬 환경에서 실행되기 때문에 실제 네트워크 지연 시간이나 에러를 정확히 반영하지 못한다는 한계가 존재했다.

> [!info]
> [실제로 json-server를 이용하여 Mock 서버를 구성한 프로젝트](https://github.com/effective-tech-interview/effective-tech-interview-client/tree/dev/mock-server/src)

그렇다고 이러한 한계를 해결하기 위해서 만약 원격 Mock 서버를 따로 구성한다면 환경 구성 작업등에 많은 비용이 들어갈 것이다.

##### 3. MSW를 이용하여 Mocking 하기
위와 같은 한계와 문제를 해결하기 위해 방법을 찾던 중 실제 API를 이용하는 것처럼 네트워크 수준에서 Mocking을 할 수 있는 [MSW(Mock Service Worker)](https://mswjs.io/)를 발견하게 되었다.

![[msw-github-logo.webp|600]]

MSW(Mock Service Worker)의 동작을 간단하게 설명하자면 서버쪽에 요청이 들어왔을 때 네트워크 요청을 가로채서 모의 응답(Mocked Response)를 보내주는 도구이다. 따로 Mock 서버를 구축하지 않아도 네트워크 수준에서 Mocking을 할 수 있기 때문에 기존 Mock Server의 한계를 해결할 수 있다.

자세한 설명은 아래의 글들을 참고하자.

> [!info]
> - [MSW - Getting started](https://mswjs.io/docs/getting-started)
> - [Mocking으로 생산성까지 챙기는 FE 개발](https://tech.kakao.com/2021/09/29/mocking-fe/)
> - [Next.js에서 MSW(Mock Service Worker)로 네트워크 Mocking하기](https://oliveyoung.tech/blog/2024-01-23/msw-frontend/)

기존 Mock Server의 장점에 추가적인 서버 구성 없이 실제 네트워크 환경에 가까운 Mocking을 사용할 수 있다는 점에서 MSW 사용을 결정하게 되었다.

#### Storybook, Jest
채용 공고에 써있던 우대 사항을 보며 과제에 녹아낼 수 있는 내용이 있는지 확인해보았고 다음과 같은 두 가지 문구를 발견하게 되었다.

![[29cm-preferential-treatment (1).webp]]

먼저 CDD를 제대로 적용한 것은 아니었지만 [이전 프로젝트](https://github.com/depromeet/pingpong-client)에서 스토리북을 사용해본적이 있었고 CDD에 대한 관심이 있었기 때문에 이것을 이번 과제에 적용해보기로 결정하였다.

CDD를 적용하기 위해 다음의 글들을 참고하였다.

> [!info]
> - [Component-Driven Development - Tom Coleman](https://www.chromatic.com/blog/component-driven-development/)
> - [당신2 9하던 Storybook](https://medium.com/29cm/%EB%8B%B9%EC%8B%A02-9%ED%95%98%EB%8D%98-storybook-a6b10a62e825)
> - [컴포넌트 주도 개발 (Component Driven Development)](https://yamoo9.github.io/react-master/lecture/sb-cdd.html)

> CDD를 어떻게 적용했는지에 대한 자세한 내용은 2부에서 다룰 예정이다.

또한 이전 프로젝트에서 작게나마 Jest와 Playwright를 사용한 경험이 있었기 때문에 이를 토대로 유닛 테스트와 e2e 테스트를 진행해보기로 했다.

## 결론
위와 같은 생각들을 바탕으로 과제를 위한 기술 스택을 선정하였다. 이번 과제 덕분에 다양한 기술의 필요성과 사용 이유를 되짚어 볼 수 있었다. **"왜 이러한 기술을 사용했는가?"** 라는 질문에 이제는 어느정도 답변을 할 수 있을 것 같다.

회고를 작성하다보니 내용이 많아져 어쩔 수 없이 1부와 2부를 나누게 되었다. 2부에서는 1부에서 채택한 기술들을 바탕으로 어떻게 과제에 진행했는지 그리고 그 과정에서 어떤 문제들을 만나 어떻게 해결했는지에 대한 내용들을 다룰 예정이다. 또한 마지막으로 과제를 진행하면서 미쳐 신경쓰지 못한 아쉬운 점 까지 종합적인 후기를 작성할 생각이다.



