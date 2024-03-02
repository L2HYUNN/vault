Vanilla Extract에 모든 styling APIs는 input으로 `style object`를 가진다.

스타일을 JavaScript object로 표현하는 것은 style이 나머지 어플리케이션과 같은 `typed data-structure`를 가지므로 코드 스타일링에 TypeScript를 더 잘 사용할 수 있게 해준다.

> [!info]
> 이것은 또한 `type-safety`와 CSS 작성에 `autocomplete`를 제공한다. (via [csstype](https://github.com/frenic/csstype)).

## CSS Properties
`style object`의 최상위에서, CSS 속성들은 일반적인 CSS class를 작성할 때와 같이 작성할 수 있다.

> [!check]
> 일반 CSS와 유일한 차이는 `kebab-case`가 아닌 `camelCase`를 사용한다는 것이다.

```tsx
// app.css.ts
import { style, globalStyle } from '@vanilla-extract/css';

export const myStyle = style({
  display: 'flex',
  paddingTop: '3px'
});

globalStyle('body', {
  margin: 0
});
```

### Unitless Properties
일부 속성들은 변수로 `number`를 사용한다. [unitless properties](https://github.com/vanilla-extract-css/vanilla-extract/blob/6068246343ceb58a04006f4ce9d9ff7ecc7a6c09/packages/css/src/transformCss.ts#L25)를 제외하고, 이 변수들은 `pixel`이 될 것이라 추정되어 `px`이 자동적으로 변수에 추가된다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

export const myStyle = style({
  // cast to pixels
  padding: 10,
  marginTop: 25,

  // unitless properties
  flexGrow: 1,
  opacity: 0.5
});
```

### Vendor Prefixes
만약 `-webkit-tap-highlight-color`와 같은 `vendor specific property`를 사용하려 하는 경우, 처음에 `-`를 제거하고 `PascalCase`를 사용할 수 있다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

export const myStyle = style({
  WebkitTapHighlightColor: 'rgba(0, 0, 0, 0)'
});
```

## CSS Variables
일반적인 CSS에서 변수 (혹은 CSS custom properties)를 규칙내의 다른 속성들과 함께 설정할 수 있다. Vanilla Extract에서, CSS 변수들은 다른 CSS 속성들에 조금 더 정확한 `static typing`을 제공하는 `vars`key 내부로 감싸져야만 한다.

```tsx
import { style } from '@vanilla-extract/css';

const myStyle = style({
  vars: {
    '--my-global-variable': 'purple'
  }
});
```

`vars` key는 또한 [createVar](https://vanilla-extract.style/documentation/api/create-var/)를 통해 생성되는 scoped CSS 변수를 허용한다.

```tsx
// style.css.ts
import { style, createVar } from '@vanilla-extract/css';

const myVar = createVar();

const myStyle = style({
  vars: {
    [myVar]: 'purple'
  }
});
```

## Media Queries
일반적인 CSS와 달리, Vanilla Extract는 `@mdeia` key를 이용해서 스타일 정의에 `media queries`를 입력할 수 있게 해준다. 이것은 하나의 데이터 구조안에 반응형 스타일 규칙을 쉽게 동일하게 위치 시킬 수 있다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

const myStyle = style({
  '@media': {
    'screen and (min-width: 768px)': {
      padding: 10
    },
    '(prefers-reduced-motion)': {
      transitionProperty: 'color'
    }
  }
});
```

> [!check]
> CSS에서 코드를 처리할 때, Vanilla Extract는 항상 파일의 끝에서 `media queries`를 랜더한다. 이것은 CSS의 우선순위 규칙에 의해 `@media` key안에 스타일들이 항상 다른 스타일들 보다 높은 우선순위를 가진다는 것을 의미한다.

이렇게 하는 것이 안전할 때 Vanilla Extract는 `@media`,` @supports`, `@container` 컨디션 블록들을 함께 합쳐 가능한 가장 작은 CSS output을 만든다.

## Selectors
주어진 스타일에 `selectors`를 지정하는 두 가지 방법이 있다. 하나는 모든 다른 CSS 속성들과 함께 사용할 수 있는 간단한 `pseudo selectors`를 이용하는 방법이고 다른 하나는 조금 더 복잡한 규칙들을 구성할 수 있는 `selectors` 옵션을 사용하는 것이다.

> [!note]
> `globalStyle`에는 모든 `selectors`을 사용할 수 없다. 이 API는 첫 번째 매개변수를 `selector`로 허용한다 `(e.g. ul li:first-of-type, a > span)`, `selectors`를 병합하면 예상치 못한 결과과 발생할 수 있다.

### Simple Pseudo Selectors
간단한 `pseudo selectors`는  어떤 매개변수도 가지지 않기 때문에 **쉽게 발견**되고 **정적으로 입력**될 수 있다. 이것은 최상위 수준에서  [CSS properties](https://vanilla-extract.style/documentation/styling/#css-properties)와 함께 사용될 수 있으며 오직  [CSS Properties](https://vanilla-extract.style/documentation/styling/#css-properties) 와 [CSS Variables](https://vanilla-extract.style/documentation/styling/#css-variables)를 포함할 수 있다.

```tsx
// style.css.ts
import { style } from '@vanilla-extract/css';

const myStyle = style({
  ':hover': {
    color: 'pink'
  },
  ':first-of-type': {
    color: 'blue'
  },
  '::before': {
    content: ''
  }
});
```

### Complex Selectors
조금 더 복잡한 규칙들은 `selectors` key를 사용하여 작성할 수 있다.

유지보수성을 향상시키기 위해, 각각의 스타일 블록들은 오직 하나의 `element`만을 대상으로 지정 할 수 있다. 이것을 강제하기 위해서 모든 `selectors`는 현재 `element`를 참조하는 `&` 문자를 대상으로 해야만한다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

const link = style({
  selectors: {
    '&:hover:not(:active)': {
      border: '2px solid aquamarine'
    },
    'nav li > &': {
      textDecoration: 'underline'
    }
  }
});
```

```css
.styles_link__1hiof570:hover:not(:active) {
  border: 2px solid aquamarine;
}
nav li > .styles_link__1hiof570 {
  text-decoration: underline;
}
```

`Selectors`는 다른 범위의 `class` 이름들을 참조할 수 있다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

export const parent = style({});

export const child = style({
  selectors: {
    [`${parent}:focus &`]: {
      background: '#fafafa'
    }
  }
});
```

```css
.styles_parent__1hiof570:focus .styles_child__1hiof571 {
  background: #fafafa;
}
```

> [!error]
> 현재 `class`가 아닌 다른 `element`를 대상으로 지정하는 `selectors`는 유효하지 않다.

```tsx
import { style } from '@vanilla-extract/css';

const invalid = style({
	selectors: {
	 // ❌ ERROR: Targetting `a[href]` 
	 '& a[href]': {...}, 
	 
	 // ❌ ERROR: Targetting `.otherClass` 
	 '& ~ div > .otherClass': {...} 
	} 
});
```

다른 범위의 클래스를 대상으로 지정하려면 대신 해당 `class`의 스타일 블록 내에 정의해야 한다.

```tsx
import { style } from '@vanilla-extract/css'; 

// Invalid example: 
export const child = style({}); 
export const parent = style({ 
	selectors: { 
	// ❌ ERROR: Targetting `child` from `parent` 
		[`& ${child}`]: {...} 
	} 
}); 

// Valid example: 
export const parent = style({}); 
export const child = style({
	selectors: { 
		[`${parent} &`]: {...} 
	} 
});
```

`'& a[href]'`와 같이 현재 요소 내부의 자식 노드를 전역에서 대상으로 지정하려는 경우,  [globalStyle](https://vanilla-extract.style/documentation/global-api/global-style)을 대신 사용할 수 있다.

```tsx
// style.css.ts
import { style, globalStyle } from '@vanilla-extract/css';

export const parent = style({});

globalStyle(`${parent} a[href]`, {
  color: 'pink'
});
```

### Circular Selectors
만약 `selectors`가 서로 의존되어 있다면, `getters`를 사용하여 그들을 정의할 수 있다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

export const child = style({
  background: 'blue',
  get selectors() {
    return {
      [`${parent} &`]: {
        color: 'red'
      }
    };
  }
});

export const parent = style({
  background: 'yellow',
  selectors: {
    [`&:has(${child})`]: {
      padding: 10
    }
  }
});
```

> [!tip]
> [[Container Query vs Media Query|Container Query vs Media Query 에 대하여]]
## Container Queries
Container Queries는 [media queries](https://vanilla-extract.style/documentation/styling/#media-queries)와 동일하게 동작하며 `@container` key로 감싼다.

> [!warning]
> 대상으로 하는 브라우저가  [container queries를 지원하는지](https://caniuse.com/css-container-queries) 확인하자. Vanilla Extract는  [container query syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries)를 지원하지만 지원되지 않는 브라우저에 기능적 `polyfill`은 아니다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

const myStyle = style({
  '@container': {
    '(min-width: 768px)': {
      padding: 10
    }
  }
});
```

 [createContainer](https://vanilla-extract.style/documentation/api/create-container/)를 사용하면 `scoped container`를 생성할 수 있다.

```tsx
// styles.css.ts
import {
  style,
  createContainer
} from '@vanilla-extract/css';

const sidebar = createContainer();

const myStyle = style({
  containerName: sidebar,
  '@container': {
    [`${sidebar} (min-width: 768px)`]: {
      padding: 10
    }
  }
});
```

```css
.styles_myStyle__1hiof571 {
  container-name: styles_sidebar__1hiof570;
}
@container styles_sidebar__1hiof570 (min-width: 768px) {
  .styles_myStyle__1hiof571 {
    padding: 10px;
  }
}
```

> [!tip]
[[layer| CSS @layer 이해하기]]
## Layers
위의 `media queries`와 마찬가지로, Vanilla Extract는 스타일 정의 내부에 `@layer` key를 사용하여  [layers](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer)에 스타일을 지정할 수 있게 해준다.

> [!warning]
> 대상으로 하는 브라우저가  [layers를 지원하는지](https://caniuse.com/css-cascade-layers) 확인하자. Vanilla Extract는  [layers syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer)를 지원하지만 지원되지 않는 브라우저에 기능적 `polyfill`은 아니다.

```tsx
// styles.css.ts
import { style } from '@vanilla-extract/css';

const text = style({
  '@layer': {
    typography: {
      fontSize: '1rem'
    }
  }
});
```

`@layer` key는 또한 [layer](https://vanilla-extract.style/documentation/api/layer/) API를 통해 생성되는 제한된 레이어 참조를 허용한다.

```tsx
// styles.css.ts
import { style, layer } from '@vanilla-extract/css';

const typography = layer();

const text = style({
  '@layer': {
    [typography]: {
      fontSize: '1rem'
    }
  }
});
```

> 레이어 관리에 대해서 조금 더 배우려면, [layer](https://vanilla-extract.style/documentation/api/layer/) 그리고 [globalLayer](https://vanilla-extract.style/documentation/global-api/global-layer/)API 문서를 살펴보자.

## Supports Queries

## Fallback Styles