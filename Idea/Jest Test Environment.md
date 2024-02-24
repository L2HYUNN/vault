[[JSDom]]

https://stackoverflow.com/questions/69227566/consider-using-the-jsdom-test-environment

Next Doc에 따르면 Jest Config에 testEnvironment를 `jsdom`으로 설정하는 것을 볼 수 있다.

Next에서는 왜 testEnvironment를 `jsdom`으로 설정하였는지 알아보고 다른 testEnvironment를 함께 살펴보자.

우리는 어떤 testEnvironment를 설정해야하는가?
## Important note for jest > 28
28 버전 이상의 Jest를 사용하는 경우 별도로 `jest-environment-jsdom`을 설치한다.

### Why?
기본적으로 Jest는 `node`를 testEnvironment로 사용한다. 
이것은 본질적으로 브라우저 환경에서의 모든 테스트를 무효화시킨다.

`jsdom`은 이러한 UI 테스트를 지원하기 위한 브라우저 환경에 대한 구현이다.

28 버전 혹은 그 이상의 Jest에서는 기본적으로 Jest 설치에 대한 패키지 사이즈를 줄이기 위해 `jest-environment-jsdom`이 빠지게되었다.

## Additional reading

[jest testEnvironment documentation](https://jestjs.io/docs/configuration#testenvironment-string)

[Jest 28 breaking changes](https://jestjs.io/blog/2022/04/25/jest-28#breaking-changes)

## Jest uses Default test environment

This can be solved on a per-test-file basis by adding a `@jest-environment` docblock to the beginning of your file. For example:

> jest 실행 환경을 `jsdom`으로 설정하지 않고도 `@jest-environmen`t doc block을 이용하면 jsdom을 이용한 test를 실행할 수 있다.

```javascript
/** @jest-environment jsdom */
import React from 'react'
import { render } from '@testing-library/react'

import Button from '.'

describe('Button', () => {
  it('renders button without crashing', () => {
    const label = 'test'

    render(<Button label={label} />)
  })
})
```

If your project has a mix of UI and non-UI files, this is often preferable to [changing the entire project](https://stackoverflow.com/a/69228464/25507) by setting `"testEnvironment": "jsdom"` within your package.json or Jest config. By skipping initializing the JSDom environment for non-UI tests, Jest can run your tests faster. In fact, that's why Jest [changed the default test environment in Jest 27](https://jestjs.io/blog/2021/05/25/jest-27#flipping-defaults).

> 만약 프로젝트에 UI 와 non-UI 파일들이 섞여있다면, 종종 package.json 혹은 Jest Config에 `"testEnvironment": "jsdom"`을 설정하여 모든 프로젝트를 변경하는 것이 선호된다. non-UI 테스트를 위해 **jsdom 환경 구성을 스킵**함으로써 jest는 더욱 빠른 테스트를 실행할 수 있다. 이러한 이유에서 Jest는 Jest 27 버전에서 기본 테스트 환경을 jsdom에서 `node`로 변경하였다.


Running tests in a [JSDOM environment](https://jestjs.io/docs/configuration#testenvironment-string) incurs a significant performance overhead. Because this was the default behavior of Jest unless otherwise configured up until now, users who are writing Node apps, for example, may not even know they are given an expensive DOM environment that they do not even need.  

> JSDOM 환경에서 테스트를 실행하는 것은 상당한 퍼포먼스 오버헤들르 발생시킨다. Jest에서는 지금까지 다른 설정을 하지 않는 경우 기본적으로 JSDOM 환경에서 실행됐기 때문에 예를들어 Node 앱을 사용하는 유저의 경우 그들이 필요로 하지 않는 비싼 DOM 환경을 지불하고 있음을 모를 수 있다.

For this reason, we are **changing the default test environment** from `"jsdom"` to `"node"`. If you are affected by this change because you use DOM APIs and do not have the test environment explicitly configured, you should be receiving an error when e.g. `document` is accessed, and you can configure `"testEnvironment": "jsdom"` or use [per-file environment configuration](https://jestjs.io/docs/configuration#testenvironment-string) to resolve this.  

> 이러한 이유에서 Jest 팀은 기본 테스트 환경을 `"jsdom"`에서 `"node"`로 변경하였다. 만약 DOM APi가 필요한 경우 `"testEnvironment": "jsdom"`을 config 파일에 포함하여 사용하거나 파일마다 환경 설정을 포함하여 DOM API를 사용할 수 있다.

For mixed projects where some tests require a DOM environment but others don't, we recommend using the fast `"node"` environment by default and declaring exactly those tests that need the DOM using [docblocks](https://jestjs.io/docs/configuration#testenvironment-string).  

> DOM 환경을 필요로 하는 테스트와 그렇지 않은 테스트가 섞인 프로젝트의 경우, Jest 팀에서는 기본적으로 `"node"` 환경을 사용하고 DOM 이 필요한 테스트의 경우 docblocks을 사용할 것을 권장하고 있다.

In the next major, we plan to also eliminate `jest-jasmine2` and `jest-environment-jsdom` from the Jest dependency tree and require them to be installed explicitly, so that many users can profit from a smaller install size with less clutter that they don't need.

> 위와 같은 이유에서 Jest 28 버전에서는 `jest-jasmine2` 와 `jest-environment-jsdom`이 기본적으로 설치되는 Jest 디팬던시 트리에서 제거되었다. 이것은 많은 유저들이 더 작은 인스톨 사이즈로부터 이득을 얻게 될 것이다. 