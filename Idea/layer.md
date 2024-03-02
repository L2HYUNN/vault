 [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) [at-rule](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)인 `@layer`는 `cascade layer`를 선언하는데에 사용되며 여러개의 `casacade layer`가 있는 경우 우선순위를 정의하는데에 사용될 수 있다.

## Syntax

```CSS
@layer layer-name {rules}
@layer layer-name;
@layer layer-name, layer-name, layer-name;
@layer {rules}
```

- `layer-name`: cascade layer의 이름
- `rules`: cascade layer에 CSS 규칙들의 집합

## Description
`cascade`와 함께 `cascade layer` 내부의 규칙들은 웹 개발자에게 `cascade`에 대한 더 많은 제어권을 준다. 

레이어에 없는 모든 스타일들은 함께 모여 하나의 익명 레이어에 위치되며 이름이 있거나 익명으로 선언된 모든 레이어 뒤에 오게된다. 이것은 레이어 외부에 선언된 모든 스타일들이 특이성과 상관없이 레이어에 선언된 스타일들을 **재정의**한다는 것을 의미한다.

`@layer`는 다음의 3가지 방법 중에 하나로 `cascade layer`를 생성하는데 사용되어진다.

1. named cascade layer with CSS rules
```css
@layer utilities {
  .padding-sm {
    padding: 0.5rem;
  }

  .padding-lg {
    padding: 0.8rem;
  }
}
```

2. named cascade layer without CSS rules
```css
@layer utilities;
@layer theme, layout, utilities;
```

> [!tip]
> 이것은 레이어들이 선언된 초기 순서가 레이어가 **우선순위**를 가지는 것을 나타내기 때문에 유용하다. 다수의 레이블을 선언할 때 리스트의 마지막 레이어는 더 큰 우선순위를 가진다. 따라서 위의 예시에서 `utilities` > `layout` > `theme` 순서로 우선순위를 가진다. (`layer`는 `specificity`와 `appearance`의 순서를 무시한다.)

3.anonymous cascade layer
```css
@layer {
  p {
    margin-block: 1rem;
  }
}
```

> [!info]
> `anonymous cascade layer`는 `named layer`와 같은 방식으로 동작 하지만 이후에 규칙을 할당할 수 없다. 익명 레이어의 순서는 레이어가 선언된 순서를 따른다. 레이어 외부에 선언된 스타일 보다는 낮은 우선순위를 가진다

 > [!todo]
 > `import` 부분 부터 추가 필요

