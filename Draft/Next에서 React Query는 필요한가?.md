![[next-app-router.webp|]]

Next는 [13 버전](https://nextjs.org/blog/next-13)에서 서버 컴포넌트를 이용한 App Router를 발표하였다.

App Router에서 주목할 부분은 바로 서버 컴포넌트 아키텍쳐를 적용했다는 점이다.

> [!tip]
> 서버 컴포넌트에 대한 자세한 내용은 아래의 글들을 참고하자.
> - [React 18: 리액트 서버 컴포넌트 준비하기](https://tech.kakaopay.com/post/react-server-components/)
> - [새로 등장한 '리액트 서버 컴포넌트' 이해하기](https://yozm.wishket.com/magazine/detail/2271/)
> - [React 서버 컴포넌트를 사용 해야 하는 이유와 방법](https://www.freecodecamp.org/korean/news/how-to-use-react-server-components/)
> - [Next) 서버 컴포넌트(React Server Component)에 대한 고찰](https://velog.io/@2ast/React-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8React-Server-Component%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)

Next는 이러한 서버 컴포넌트를 이용하여 간단하게 Data fetching을 할 수 있는 기능을 제공하고 있다.

이전까지 우리는 서버 상태 관리라는 목적하에 React Query, SWR과 같은 도구들을 이용해왔다. Next 13 이전 까지 데이터 패칭을 위한 별도의 방법을 제공하고 있지 않았기 때문에 우리는 Next에서 자연스럽게 서버 상태 관리 라이브러리를 이용해왔다.

하지만 이제는 Next에서 자체적으로 데이터 패칭을 위한 기능을 제공해주고있다. 이러한 상황에서 우리는 이제 React Query가 필요한가? 에 대해서 생각해볼 필요가 있다. 




Next14에서 서버 컴포넌트를 사용할 수 있게 되면서 Next 자체적으로 외부 데이터를 위한 API들을 제공하고 있다. 이러한 상황에서 React Query가 Next 14에서 가지는 의미를 살펴보고 그 사용 방법 또한 알아본다.