일부 컴포넌트는 외부 시스템에 동기화할 필요가 있다. 예를들어, React 상태를 기반으로 non-React 컴포넌트를 제어하거나 서버 연결을 설정하거나 혹은 컴포넌트가 화면에 나타날 때 분석 로그를 보내는 것을 원할 수 있다. 특별한 이벤트를 다룰 수 있게 해주는 event handlers와 달리 Effects는 랜더링 이후에 코드를 실행할 수 있게 해준다. 그것을 이용하여 React의 외부 시스템에 컴포넌트를 동기화할 수 있다. 

## What are Effects and how are they different from events?