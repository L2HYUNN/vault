### Installation
```shell
npm install @vanilla-extract/css
```

### Bundler Integration
**Bundler**를 설정하고 CSS를 다루기위한 구성을 해야한다.
이를 통해 다른 종속성처럼 스타일을 다룰 수 있으며 필요한 부분만 **importing**, **bundling** 할 수 있다.

- [vite](https://vanilla-extract.style/documentation/integrations/vite/)
- [esbuild](https://vanilla-extract.style/documentation/integrations/esbuild/)
- [webpack](https://vanilla-extract.style/documentation/integrations/webpack/)
- [next](https://vanilla-extract.style/documentation/integrations/next/)
- [parcel](https://vanilla-extract.style/documentation/integrations/parcel/)
- [rollup](https://vanilla-extract.style/documentation/integrations/rollup/)
- [gatsby](https://vanilla-extract.style/documentation/integrations/gatsby/)

### Create a style
`.css.ts` 형식의 파일을 통해 **stylesheet**을 생성할 수 있다.

```tsx
import { style } from '@vanilla-extract/css';

export const container = style({
  padding: 10
});
```

> [!note]
> **locally scoped class**를 만들고, **class name**을 **export**한다.

### Apply the style

1. Element에 적용할 **stylesheet**을 import한다.
2. Element의 **class** 속성을 통해 stylesheet을 적용한다.

> [!info]
> style을 import함으로써 우리는 **scoped class name**을 제공받는다.

```tsx
import { container } from './styles.css.ts';

document.write(`
  <section class="${container}">
    ...
  </section>
`);
```

> [!caution] 
> import의 side effect로 CSS는 선택한 **bundler integration**을 통해 처리된다.





