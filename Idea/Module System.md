## Module
모듈이란 **여러 기능들에 관한 코드가 모여있는 하나의 파일** 로 다음과 같은 것들을 위해 사용한다.

- **유지보수성** : 기능들이 모듈화가 잘 되어있다면, 의존성을 그만큼 줄일 수 있기 때문에 어떤 기능을 개선한다거나 수정할 때 훨씬 편하게 할 수 있다.
- **캡슐화**: 각 모듈들은 그것의 기능별로 캡슐화를 할 수 있다. 이를 통해 전역 범위의 코드를 깔끔하게 만들고 변수의 이름 충돌을 줄일 수 있다.
- **네임스페이스화** : 자바스크립트에서 전역변수는 전역공간을 가지기 때문에 코드의 양이 많아질수록 겹치는 네임스페이스가 많아질 수 있다. 그러나 모듈로 분리하면 모듈만의 네임스페이스를 갖기 때문에 그 문제가 해결된다.
- **재사용성** : 똑같은 코드를 반복하지 않고 모듈로 분리시켜서 필요할 때마다 사용할 수 있다.

이런 장점들을 살리기 위해서 모듈 개념이 필요했고 자바스크립트에선 모듈을 개발하기 위한 여러가지 시도들이 있었다. CommonJS, AMD, UMD 및 ES6 등 각각의 특징과 사용법을 알아보자.

## CommonJS

> [!tip]
> 자바스크립트의 공식 스펙이 브라우저만 지원했기 때문에 이를 서버사이드 및 데스크탑 어플리케이션에서 지원하기 위한 노력이 있었다. 그걸 위해 만든 그룹이 CommonJS이며 여기선 자바스크립트가 범용적인 언어로 쓰이기 위한 스펙을 정의하고 있다. 그룹을 만들었을 때, 범용적인 언어로 만들기 위해서는 모듈화의 개념이 필요했고 이 그룹만의 모듈 방식을 정의하게 되었는데 그것이 바로 CommonJS 방식의 모듈화다.

다른 모듈을 사용할 때는 **require** 를, 모듈을 해당 스코프 밖으로 보낼 때에는 **module.exports** 를 사용하는 방식으로, Node.js에선 현재 이 방식을 사용하고 있다.

> [!note]
> Node v12부터는 기본적으로 ES6 모듈을 지원하고 있다. 하지만 파일 경로 구문에 차이가 있다.

### Example

- `a.js`

```js
const printHelloWorld = () => {
  console.log('Hello Wolrd');
};

module.exports = {
  printHelloWorld
};
```

- `b.js`

```js
const func = require('./a.js'); // 같은 디렉토리에 있다고 가정
func.printHelloWorld();
```

여기서 `module.exports` 의 `module` 은 현재 모듈에 대한 정보를 갖고 있는 객체이다. 이는 예약어이며 그 안에 `id` , `path` , `parent` 등의 속성이 있고 `exports` 객체를 가지고 있다.

### exports vs module.exports
`module.exports` 외에도 `exports` 를 사용하기도 하는데 이 관계에 대해서 명확히 이해하고 있어야 한다. 정리하자면 아래와 같다.

- `module.exports` 는 빈 객체를 참조한다.
- `exports` 는 `module.exports` 를 참조한다.
- `require` 는 항상 `module.exports` 를 리턴받는다.

즉, 함수를 모듈 밖으로 내보내고자 할 때는 위에 예시에서 2가지 모두 사용할 수 있다.

```js
exports.printHelloWorld = printHelloWorld;
module.exports = { printHelloWorld };
```

그렇다면 왜 2가지를 설정해놓았을까? 그 이유는 `exports` 는 항상 `module.exports` 를 참조하기 때문에 `exports` 를 사용하면 직접 `module.exports` 를 수정하지 않고 객체의 멤버를 만들거나 수정하는 방식으로 사용한다. 따라서, `exports` 에 어떤 값을 할당하거나 새로운 객체를 할당했다고 하더라도 결국 `require` 는 `module.exports` 를 리턴 받기 때문에 잠재적인 버그를 피할 수가 있다.

## AMD (Asynchronous Module Definition)
> CommonJS가 서버쪽에서 장점이 많은 반면에 AMD는 브라우저 쪽에서 더 큰 효과를 발휘한다. 

브라우저에서는 모든 모듈이 다 로딩될 때까지 기다릴 수 없기 때문에 **비동기 모듈 로딩방식**으로 구현했다

이 방식에서 사용하는 함수는 `define()` 과 `require()` 이며 AMD 스펙을 가장 잘 구현한 모듈로더는 **RequireJS** 이다. 
## UMD (Universal Module Definition)
위에서 살펴본 바로, 모듈 구현방식이 CommonJS 와 AMD로 나뉘기 때문에 그걸 통합하기 위한 하나의 패턴이라고 할 수 있다.

 [공식 umd 소스코드](https://github.com/umdjs/umd/blob/master/templates/returnExports.js) 를 살펴보면 아래와 같다.

```js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        // Node. Does not work with strict CommonJS, but
        // only CommonJS-like environments that support module.exports,
        // like Node.
        module.exports = factory();
    } else {
        // Browser globals (root is window)
        root.returnExports = factory();
  }
}(typeof self !== 'undefined' ? self : this, function () {

    // Just return a value to define the module export.
    // This example returns an object, but the module
    // can return a function as the exported value.
    return {};
}));
```

AMD, CommonJS, Browser 방식의 모듈을 지원하기 위한 것으로 확인하는 방식은 코드를 살펴보면 다음과 같다.

- **AMD**: `define()` 이 함수이고 `define.amd` 속성의 객체를 가지고 있다.
- **CommonJS**: `module` 이 객체이고 `module.exports` 속성의 객체를 가지고 있다.
- **Browser**: 따로 특이사항이 없다.

통합하는 방식은 2개의 인자를 전달받는 함수를 실행하는 것으로, 첫번째 인자는 Browser 쪽을 구현할 `root` 에 넘길 값으로 `undefined` 이면 `this` 로 아니라면 `self` , 즉 `window` 로 설정한다. 그리고 2번째 인자로 빈 객체 리터럴을 리턴하는 함수를 보낸다. 이렇게 되면 각각의 환경에서 모두 모듈개념을 사용할 수 있게 된다.

## ES6(ES2015)
`import` 와 `export` 구문을 사용하는 방식

하지만 모든 브라우저가 지원하는 것이 아니기 때문에 Babel의 `@babel/plugin-transform-modules-commonjs` 를 통해 변환시켜서 사용한다.

### Example

- `moduleA.js`

```js
const A = () => {};
export default A;
```

- `moduleB.js`

```js
export const B = () => {};
```

- `index.js`

```js
import A from 'moduleA';
import { B } from 'moduleB';
```

여기서 눈여겨봐야될 것은 export의 `default` 의 유무이다.

`export` 를 사용할 때는 **named export** 와 **default export** 를 사용할 수 있다. 

> [!tip]
> default export는 모듈 내에서 한번만 사용할 수 있고 named export는 여러번 사용할 수 있다. 

### How
- default export로 내보내면 `import` 에서 내보낸 이름 그대로 사용할 수 있다.
- named export로 내보내면 `{}` 로 묶어서 사용해야한다. 

### ETC
별칭(alias)을 `as` 로 주어서 다른 이름으로 사용할 수도 있고 `*` 와일드카드를 사용하여 한번에 불러오거나 내보낼 수도 있다. 이런 여러가지 변형기법의 사용은 [여기](https://velog.io/@doondoony/JavaScript-Module-System#-es6-modulesesm) 를 참고하자.
## Reference
[Must Know About Frontend / javascript / module](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/module.md)
