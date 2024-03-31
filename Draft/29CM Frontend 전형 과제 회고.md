
![[29cm-logo.webp]]

> [!info]
> 본 글은 3월에 진행했던 **29CM Frontend Eginner** 전형 과제에 대한 회고입니다. 
> 모든 과제 내용은 **외부 유출이 금지**되어 있기 때문에 공개할 수 없는 점 양해 부탁드립니다.

본격적인 취업 준비를 하기 시작하면서 취업 공고나 정보를 얻을 수 있는 원티드, 프로그래머스, 로켓펀치, 카카오톡 오픈채팅방 등을 확인하는 것이 하루 일과가 되었다.

당시 이미 여러 기업에 지원을 했던 상태였고 운좋게 면접까지 이어진 곳 또한 있었다. 하지만 아쉽게도 최종 합격까지는 이어지지 못했고 그 과정속에서 얻은 경험을 바탕으로 부족한 부분을 채워가고 있었다.

이렇게 본격적으로 채용 과정을 경험하면서 혼자 준비할 때는 몰랐던 단점들을 발견할 수 있었다. 떨어지는 것은 분명 뼈 야픈 경험이었지만 이를 통해 몰랐던 단점들을 보완할 수 있는 기회를 얻었기 때문에 좌절 보다는 오히려 기회를 얻었다고 생각했다.

이러한 상황 속에서 우연히 **29CM Frontend Enginner** 공고를 보게되었다. 29CM는 개인적으로 꼭 가고 싶다고 생각한 기업이었기 때문에 굉장히 설레는 마음으로 채용 공고를 확인해보았다.

![[29cm-frontend-condition.webp|600]]

프론트엔드 개발 경력이 2년 이상이라는 부분에서 신입은 지원할 수 없다고 생각했지만.. 아래의 한 줄이 있어 29CM에 지원을 결심하게 되었다.

> **"29CM는 경력보다 역량을 중요하게 생각하고 있습니다. 경력기간 요건에 부합하지 않더라도 아래 자격요건에 부합하시는 경우 지원 가능합니다."**

TODO: 부족한 부분이 많지만 지원이라도 해보자.

## 서류 합격
서류 지원을 마치고 얼마나 시간이 지났을까 지원에 대한 기억이 희미해질 즈음 서류 전형에 대한 결과를 받아볼 수 있었다.

![[29cm-document-passed.webp|600]]

사실 자격 요건에 경력 2년 이상이라는 부분이 있었기 때문에 지원을하면서도 합격을 기대하기는 어렵다고 생각했다. 서류에 대한 기준이 그만큼 높다고 생각했기 때문이다. 

그렇기에 크게 기대를 하지 않고 있었는데 서류 전형을 합격하게 되다니.. 어안이 벙벙했다. 운이 매우 좋다고 생각했고 이렇게 기회가 주어진 만큼 할 수 있는 모든 노력을 다해 과제를 진행해야 겠다고 생각했다.

> [!tip]
> 혹시나 서류 지원에 사용했던 이력서의 내용이 궁금하신 분들을 위해 부족하지만 지원에 사용했던 이력서 및 포트폴리오와 이력서 작성에 대한 글을 남긴다.
> - [신입 프론트엔드 개발자 이력서 작성하기](https://l2hyunn.github.io/posts/frontend-developer-resume/)
> - [이동현 이력서](https://team-project22.notion.site/Developer-699a7e05c9f3414088517e3ab3220618)
> - [이동현 포트폴리오](https://team-project22.notion.site/7190b4b78ce540ff97fea14e58aaa3c3?v=fd78ab97bc8345f7a001422cf111a9cf)

## 과제 전형

### 기술 선정
과제를 진행하기 전 처음에 가장 고민했던 것은 **"어떤 기술을 사용해야 하는가?"** 이다. 아래의 사진은 채용 공고에 나와 있는 29CM Frontend 팀이 사용하고 있는 주요 기술 스택 및 도구이다.

![[29cm-frontend-team-stack.webp|600]]

기존에 사용해왔던 기술들을 이용한다면 보다 쉽게 과제를 진행할 수 있겠지만, 가능하면 29CM Frontend 팀에서 사용하고 있는 기술들을 이용하여 과제를 진행하는 것이 좋다고 생각했다. 또한 다음과 같은 이유들에서 아래와 같은 기술들을 사용하기로 결심하였다.

#### Next 14
작년 6월 Next 13(**Page Router**)을 이용하여 진행했던 [기술 면접 대비 플랫폼](https://github.com/effective-tech-interview/effective-tech-interview-client)을 마지막으로 새로운 프로젝트를 진행하지 않았다. 새로운 프로젝트 보다는 자료구조, 알고리즘이나 자바스크립트 공부에 집중 하였고 그러다보니 새로운 버전의 Next가 나왔지만 아직 제대로 사용해본 적이 없던 상황이었다. 

![[next-app-router.webp|600]]

Next 13에서 기존의 Page Router가 [**App Router**](https://nextjs.org/docs/app)로 변경 되면서 많은 변화가 있었기 때문에 언제까지 Next의 최신 버전을 사용해보지 않을 수는 없다고 생각했다. 그렇다보니 이번에 새로운 개인 프로젝트를 기획 하면서 Next 14를 사용하려고 계획하고 있던 상황이었다.

이러한 상황 속에서 29CM Frontend 팀이 Next 14를 사용하고 있다는 것을 채용공고를 통해 알게 되었고 과제 전형에서 Next 14의 사용을 고려해보게 되었다.

Next를 이용하면 **직관적인 라우팅 시스템, 다양한 랜더링 전략, 캐싱, 이미지와 폰트 최적화** 등 고성능의 웹 어플리케이션을 위한 다양한 기능들을 사용할 수 있기 때문에 Next 사용을 결정하는데는 큰 고민이 없었다. 하지만 App Router의 사용에는 고민이 있었다. 

App Router는 기본적으로 [**서버 컴포넌트**](https://www.patterns.dev/react/react-server-components/)를 사용하기 때문에 이에 대한 충분한 이해가 필요했고 이에 따른 App Router의 전체적인 기능(공유되는 레이아웃, 중첩 라우팅, 로딩 상태, 에러 핸들링등)을 이해할 필요가 있었다. 하지만 그럼에도 서버 컴포넌트가 [다음과 같은 이점](https://nextjs.org/docs/app/building-your-application/rendering/server-components)**(Zero Bundle Size, Automatic Code Splitting, Progressive Rendering)** 을 가지기 때문에 더 나은 성능의 어플리케이션을 만들기 위해 Next 14의 사용을 결정하게 되었다.

> [!info]
> 서버 컴포넌트에 대한 자세한 설명은 [다음의 글](https://nextjs.org/docs/app/building-your-application/rendering/server-components)을 참고하자.
 
#### React-Query 
Next 14에서 서버 컴포넌트를 도입하면서 이를 통해 서버에서 직접 데이터를 요청할 수 있게 되었다. 또한 Next는 기존의 fetch Web API를 확장하여 캐싱(caching)과 재검증(revalidating)이 가능하게 만들었다. 자세한 내용은 아래의 공식 문서에 대한 설명에서 찾아볼 수 있다.

> [!note]
> [Data Fetching, Caching and Revalidating](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating)

![[react-query-v5.webp|600]]

이러한 상황에서 클라이언트 서버 상태 관리를 위해 굳이 React Query를 사용해야 하는지 의문이 생겼고 이러한 의문을 풀기 위해 다양한 자료를 찾아보았다. 

찾아본 자료들에 따르면 Next 14에서 [Server Action](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)이 안정화 되면서 서버에서 모든 데이터를 다룰 수 있기 때문에 React Query의 사용이 불필요하다고 이야기하는 것 같았지만 [특별한 경우](https://www.reddit.com/r/nextjs/comments/19d0sar/comment/kj2pdiq/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) (인피니티 스크롤, 폴링) 등을 구현할 때 도움이 될 수 있다고 이야기하는 것 같았다.

> [!info]
> - [Nextjs 14 Server actions vs React query](https://www.reddit.com/r/nextjs/comments/19d0sar/nextjs_14_server_actions_vs_react_query/)
> - [You Might Not Need React Query](https://tkdodo.eu/blog/you-might-not-need-react-query)
> - [Why I don't use React-Query and tRPC anymore](https://www.youtube.com/watch?v=51pf_nCJpwg&t=6s)

처음에는 Next에서 이미 데이터 패칭을 위한 충분한 기능들을 제공하고 있기 때문에 React Query를 사용하지 않으려 했다. 하지만 현재 Next의 App Router를 처음 사용한다는 상황을 고려해보았을 때 기존에 이용해왔던 React Query를 이용하여 서버 상태를 관리하는 것이 현재 상황에 맞는 기술 선택이라고 생각했다. 또한 결정적으로 29CM Frontend 팀에서 React Query를 사용하고 있기 때문에 React Query를 사용하기로 결정하였다.

#### Zustand
이전에 프로젝트를 진행할 때 클라이언트 상태 관리를 위해 주로 [recoil](https://recoiljs.org/ko/)을 사용하고 있었다. 처음 배웠던 상태 관리 라이브러리이기도 했고 가볍고 쉽게 사용할 수 있기 때문에 주력으로 사용했던 것 같다. 한 번즈음은 클라이언트 상태 관리를 비교해보고 장단점을 정리하여 상황에 맞는 라이브러리를 채택할 수 있어야 한다고 생각했는데 이번 기회에 이러한 비교를 해볼 수 있게 된 것 같다.

먼저 이러한 상태 관리로 가장 널리 쓰이는 redux와 같은 라이브러리는 스토어(store) 구성을 위해 많은 보일러 플레이트를 작성해야 되기 때문에 선택지에서 제외하고 생각했다.

아래의 글을 통해 상태 관리 라이브러리들의 특징과 장단점을 쉽게 정리할 수 있었다.

#### MSW

#### ETC





채용 공고에 나와있는 기술들을 과제에서 진행하기 위해서는 개인적으로 나에게 3가지 정도의 과제가 있었다.



이런 상황이다보니 과제를 진행하기 위해 어떤 기술들을 사용해야할지 고민이 많았다. 물론 익숙한 것은 이전에 사용했던 기술들이었지만 가능하면 29CM에 사용하고 있는 기술들을 사용해서 과제를 진행하고 싶다는 욕심이 들었다.


## 기술적 도전?
- Hydration Provider
- Layer Architecture
- SOLID
- CDD / TDD
- MSW


## 문제 해결

## 아쉬웠던 부분
