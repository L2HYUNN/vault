현재 예제어서 javascript만을 사용하여 main function에서 모든 것을 다루고 있기 때문에 수정시마다 다음의 작업이 반복되고 있다.

1. 데이터 패칭 (캐싱되지 않으며 파일이 수정될 때마다 매번 호출된다)
2. 전체 엘리먼트 리로딩 (변경 사항이 없는 요소들도 전부 리로딩 된다)

```js
import test from "./test.json?raw"
```

`vite`의 기능중에 하나로 `?raw` 를 붙이면 파일의 내용을 읽어 텍스트로 담아주는 문법이다.

> [!tip]
> 컴포넌트 리로딩과 반복되는 데이터 패칭에서 오는 DX 저하를 인식하고
> 이를 개선할 수 있는 방법을 항상 고민하라


