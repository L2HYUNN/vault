`build`시 `minify` 옵션을 비활성함으로써 빌드된 Javascript를 확인할 수 있다.

webpack이 아닌 vite를 사용하는 이유는 vite의 소개에서 찾아볼 수 있다. 

특히 dev server의 HMR 문제를 해결하였다. 

dev 모드에서는 변경된 파일만 새로 랜더링한다 -> 속도 향상

production에서는 기존과 같이 번들링하는데 자세한 설명은 vite 문서를 참조한다.