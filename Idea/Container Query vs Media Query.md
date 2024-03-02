`Container Query`를 이용하면 `Media Query`와 같이 **반응형 스타일**을 지정할 수 있다.

둘의 작동 방식의 차이점은 **어느 것을 기반으로 동작하느냐** 이다.

`Media Query`는 디바이스 또는 미디어 유형을 기반으로 **뷰포트**에 반응한다.

`Container Query`는 페이지내의 **특정 컴포넌트 요소 기반**으로 반응한다.

![[container-vs-media.png|600]]

> [!note]
> `Container Query`이전에는 JS의 `Resize Observer`을 통해 요소의 크기 변화를 관찰해야만 했다.

## Props
**뷰포트**가 아닌 **특정 컴포넌트를 기반**으로 반응형 스타일을 적용할 수 있기 때문에 조금 더 직관적이고 유지보수 좋은 코드를 작성할 수 있다.

## How to use
1. 반응형을 적용할 요소에 컨테이너 등록하기
```css
div {
  container-name: div-container; /* 컨테이너 쿼리 요소 이름 */
}
```
2. 컨테이너 타입 지정하기
	- `inline-size`: 인라인 레벨 기준으로 컨테이너를 적용. 요소의 `width` 값에 따라 반응형이 동작된다.
	- `size`:  블록 레벨 기준으로 컨테이너를 적용. `width` 뿐만 아니라 `height` 값에 따라 반응형이 동작 된다.
	- `normal`: 해당 값이 부여된 요소를 container에서 제외시킨다. 일종의 none 의미

```css
div {
    container-name: div-container;
    container-type: inline-size; /* 왠만한 상황에선 inline-size 을 이용 */
}
```

```css
div {
    /* container-name / container-type */
    container : div-container / inline-size; /* 한줄로도 단축 표현이 가능하다 */
}
```
3. `@container` 반응형 지정
```css
/* 모든 컨테이너 요소에 반응 */
@container (min-width: 700px) {
  div {
    font-size: 2em;
  }
}

/* 특정 container-name의 요소에 반응 */
@container div-container (min-width: 700px) {
  div {
    font-size: 2em;
  }
}
```

## Units
viewport의 `vw`, `vh` 단위 처럼 container의 너비와 높이를 기준으로 하는 상대 기준점 단위가 존재한다.
- `cqw` : 쿼리 컨테이너 너비의 1%
- `cqh` : 쿼리 컨테이너 높이의 1%
- `cqi` : 쿼리 컨테이너 인라인 크기의 1%
- `cqb` : 쿼리 컨테이너의 블록 크기의 1%
- `cqmin` : `cqi` 또는 `cqb` 중 더 작은 값
- `cqmax` : `cqi` 또는 `cqb` 중 더 큰 값

```css
/* 컨테이너의 인라인 크기를 기준으로 제목의 글꼴 크기를 설정 */
@container (min-width: 700px) {
  div {
    font-size: 1em + 2cqi;
  }
}
```

## Support
`IE`를 제외한 PC, Mobile 브라우저에서 대부분 지원하고 있다.

![[container-query-support.png|600]]

## Reference
[https://inpa.tistory.com/entry/🌟-css-container-사용법](https://inpa.tistory.com/entry/%F0%9F%8C%9F-css-container-%EC%82%AC%EC%9A%A9%EB%B2%95) [Inpa Dev 👨‍💻:티스토리]