[jest-dom v6.0.0 Releases](https://github.com/testing-library/jest-dom/releases/tag/v6.0.0)
# [6.0.0](https://github.com/testing-library/jest-dom/compare/v5.17.0...v6.0.0) (2023-08-13)

### Features

- local types, supporting jest, @jest/globals, vitest ([#511](https://github.com/testing-library/jest-dom/issues/511)) ([4b764b9](https://github.com/testing-library/jest-dom/commit/4b764b9f6a7b564d7f8ec0e9b0c6ba9cc875f2b8))
### BREAKING CHANGES

- Removes the extend-expect script. Users should use  
    the default import path or one of the new test platform-specific  
    paths to automatically extend the appropriate "expect" instance.

> extend-expect 스크립트를 제거함. 유저들은 적절한 "expect" 인스턴스를 자동으로 확장하기 위해서 기본으로 설정된 import path 또는 새로운 테스트 플랫폼 path들 중 하나를 사용해야만한다.

extend-expect was not documented in the Readme, so this change should  
have minimal impact.

> extend-expect는 Readme에 포함되지 않은 사양이기 때문에 이 변화가 작은 영향을 끼칠 것이라 판단

Users can now use the following import paths to automatically extend  
"expect" for their chosen test platform:

- @testing-library/jest-dom - jest (@types/jest)
- @testing-library/jest-dom/jest-globals - @jest/globals
- @testing-library/jest-dom/vitest - vitest

For example:

import '@testing-library/jest-dom/jest-globals'

Importing from one of the above paths will augment the appropriate  
matcher interface for the given test platform, assuming the import  
is done in a .ts file that is included in the user's tsconfig.json.

It's also (still) possible to import the matchers directly without  
side effects:

import * as matchers from '@testing-library/jest-dom/matchers'

- Update kcd-scripts
- Drop node < 14

If you need to add more setup options before each test, you can add them to the `jest.setup.js` file above.