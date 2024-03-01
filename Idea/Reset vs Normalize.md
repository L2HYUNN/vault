브라우저 마다 **User Agent Stylesheet**이라고 불리는 브라우저 내장 스타일을 제공한다.
**Reset CSS** 혹은 **Normalize CSS**를 사용하면 브라우저와 상관없는 동일한 사용자 스타일을 제공할 수 있다.

## Preview

![[reset-vs-normalize-css.png|500]]

## Reset
**Reset CSS**의 경우 브라우저의 내장 스타일을 거의 모두 초기화시킨다.

> [!tip]
>  전통적으로 [Eric Meyer’s CSS Reset](https://meyerweb.com/eric/tools/css/reset/)이 많이 사용되었지만, 최근에는 브라우저 간 호환성이 좋아지면서 불필요한 부분을 줄이고 최신 웹 개발 트랜드를 반영한 [Elad Shechter’s CSS Reset](https://elad2412.github.io/the-new-css-reset/)이 많이 사용되고 있다.

## Normalize
**Normalize CSS**의 경우 브라우저의 내장 스타일을 최대한 유지하는 선에서 브라우저 간 상이한 부분만 통일해준 스타일을 제공한다.

> [!tip]
 > [Normalize.css](https://necolas.github.io/normalize.css/)가 대중적으로 사용되고 있다.

## Conclusion
브라우저의 내장 스타일을 최대한 따르며 최소한의 변화만 주고 싶다면 **Normalize CSS**를 사용한다.
브라우저의 내장 스타일에 의존하지 않고 모든 것을 스타일하고자 한다면 **Reset CSS**를 사용한다.

## Reference
[# CSS Normalize와 CSS Reset](https://www.daleseo.com/css-normalize-reset/)
[# reset이냐 normalize냐, 그것이 문제로다!](https://velog.io/@nalsae/%EB%82%B4%EB%B3%B4%EC%A0%95CSS-reset-%EC%A4%84%EA%B9%8C-normalize-%EC%A4%84%EA%B9%8C)

