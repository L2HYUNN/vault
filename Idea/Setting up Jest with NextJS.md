[Setting up Jest with Next.js](https://nextjs.org/docs/app/building-your-application/testing/jest)

Jest and React Testing Library are frequently used together for **Unit Testing** and **Snapshot Testing**.

> Jest, React Testing Library는 **유닛 테스트**와 **스냅샷 테스트**를 위해 함께 빈번히 사용되는 라이브러리

**Good to know:** Since `async` Server Components are new to the React ecosystem, Jest currently does not support them. While you can still run **unit tests** for synchronous Server and Client Components, we recommend using an **E2E tests** for `async` components.

> Jest가 현재 `async` 서버 컴포넌트에 대한 지원을 하지 않고 있기 때문에 `async` 컴포넌트에 대해서는 **E2E** 테스트를 대신 진행할 것을 권장한다.

## [Quickstart](https://nextjs.org/docs/app/building-your-application/testing/jest#quickstart)
You can use `create-next-app` with the Next.js [with-jest](https://github.com/vercel/next.js/tree/canary/examples/with-jest) example to quickly get started:

Terminal

```
npx create-next-app@latest --example with-jest with-jest-app
```

## [Manual setup](https://nextjs.org/docs/app/building-your-application/testing/jest#manual-setup)
Since the release of [Next.js 12](https://nextjs.org/blog/next-12), Next.js now has built-in configuration for Jest.

> Next 12 버전부터 현재 Jest를 위한 빌트인 설정을 제공하고 있다.

To set up Jest, install `jest` and the following packages as dev dependencies:

> Jest 셋업을 위해 `Jest`와 아래의 패키지들을 dev 디팬던시로 설치한다.

Terminal

```
npm install -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
# or
yarn add -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
# or
pnpm install -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
```

> Test를 위해 아래의 라이브러리들을 설치하고 있다.
> 1. **jest** 
> 2. **jest-environment-jsdom** 
> 3. **@testing-library/react** 
> 4. **@testing-library/jest-dom**

Generate a basic Jest configuration file by running the following command:

> 아래의 커맨드를 이용하여 **Jest 설정 파일**을 생성한다.

Terminal

```
npm init jest@latest
# or
yarn create jest@latest
# or
pnpm create jest@latest
```

This will take you through a series of prompts to setup Jest for your project, including automatically creating a `jest.config.ts|js` file.

> prompt와 설정을 위한 `jest.config.ts` 파일을 생성한다.

Update your config file to use `next/jest`. This transformer has all the necessary configuration options for Jest to work with Next.js:

> `next/jest`를 사용하기 위해 아래와 같이 config 파일을 업데이트한다. 
> 이 트랜스포머는 Next와 함께 Jest를 위한 설정 옵션들을 모두 제공해준다. 

```TypeScript
// jest.config.ts
import type { Config } from 'jest'
import nextJest from 'next/jest.js'
 
const createJestConfig = nextJest({
  // Provide the path to your Next.js app to load next.config.js and .env files in your test environment
  dir: './',
})
 
// Add any custom config to be passed to Jest
const config: Config = {
  coverageProvider: 'v8',
  testEnvironment: 'jsdom',
  // Add more setup options before each test is run
  // setupFilesAfterEnv: ['<rootDir>/jest.setup.ts'],
}
 
// createJestConfig is exported this way to ensure that next/jest can load the Next.js config which is async
export default createJestConfig(config)
```

> [[Jest Test Environment에 대하여]]

Under the hood, `next/jest` is automatically configuring Jest for you, including:

- Setting up `transform` using the [Next.js Compiler](https://nextjs.org/docs/architecture/nextjs-compiler)
- Auto mocking stylesheets (`.css`, `.module.css`, and their scss variants), image imports and [`next/font`](https://nextjs.org/docs/pages/building-your-application/optimizing/fonts)
- Loading `.env` (and all variants) into `process.env`
- Ignoring `node_modules` from test resolving and transforms
- Ignoring `.next` from test resolving
- Loading `next.config.js` for flags that enable SWC transforms

> **Good to know**: To test environment variables directly, load them manually in a separate setup script or in your `jest.config.ts` file. For more information, please see [Test Environment Variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables#test-environment-variables).

## [Optional: Handling Absolute Imports and Module Path Aliases](https://nextjs.org/docs/app/building-your-application/testing/jest#optional-handling-absolute-imports-and-module-path-aliases)
If your project is using [Module Path Aliases](https://nextjs.org/docs/pages/building-your-application/configuring/absolute-imports-and-module-aliases), you will need to configure Jest to resolve the imports by matching the paths option in the `jsconfig.json` file with the `moduleNameMapper` option in the `jest.config.js` file. For example:

> **Module Path Aliases** 를 프로젝트에 사용하고 있다면 `jsconfig.json` 파일에 있는 path 옵션에 맞는 import를 Jest가 찾을 수 있게 추가 옵션을 `jest.config.js` 파일에 `moduleNameMapper`를 통해 제공해야한다. 

```json
// tsconfig.json or jsconfig.json
{
  "compilerOptions": {
    "module": "esnext",
    "moduleResolution": "bundler",
    "baseUrl": "./",
    "paths": {
      "@/components/*": ["components/*"]
    }
  }
}
```

```js
// jest.config.js
moduleNameMapper: {
  // ...
  '^@/components/(.*)$': '<rootDir>/components/$1',
}
```

## [Optional: Extend Jest with custom matchers](https://nextjs.org/docs/app/building-your-application/testing/jest#optional-extend-jest-with-custom-matchers)

`@testing-library/jest-dom` includes a set of convenient [custom matchers](https://github.com/testing-library/jest-dom#custom-matchers) such as `.toBeInTheDocument()` making it easier to write tests. You can import the custom matchers for every test by adding the following option to the Jest configuration file:

> `@testing-library/jest-dom`는 `@testing-library/jest-dom`와 같은 편리한 custom matcher들을 포함하여 테스트를 더 쉽게 작성할 수 있게 해준다. Jest 설정 파일에 아래의 옵션을 포함하여 모든 테스트에 custom matcher들을 포함할 수 있다.

```TypeScript
// jest.config.ts
setupFilesAfterEnv: ['<rootDir>/jest.setup.ts']
```

Then, inside `jest.setup.ts`, add the following import:

```TypeScript
// jest.setup.ts
import '@testing-library/jest-dom'
```

> **Good to know:**[`extend-expect` was removed in `v6.0`](https://github.com/testing-library/jest-dom/releases/tag/v6.0.0), so if you are using `@testing-library/jest-dom` before version 6, you will need to import `@testing-library/jest-dom/extend-expect` instead.

> extend-expect가 v6.0에서 제거되었기 때문에 만약 version 6 전에 `@testing-library/jest-dom` 를 사용하고 있다면 `@testing-library/jest-dom/extend-expect` 를 대신 Import 해야한다.

[[jest-dom v6.0.0 Releases]]

If you need to add more setup options before each test, you can add them to the `jest.setup.js` file above.

> 각 테스트 전에 셋업 옵션을 더 추가 하고 싶다면, `jest.setup.js` 파일 위에 그것들을 추가할 수 있다.

## [Add a test script to `package.json`:](https://nextjs.org/docs/app/building-your-application/testing/jest#add-a-test-script-to-packagejson)

Finally, add a Jest `test` script to your `package.json` file:

```json
// package.json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

`jest --watch` will re-run tests when a file is changed. For more Jest CLI options, please refer to the [Jest Docs](https://jestjs.io/docs/cli#reference).

### [Creating your first test:](https://nextjs.org/docs/app/building-your-application/testing/jest#creating-your-first-test)

Your project is now ready to run tests. Create a folder called `__tests__` in your project's root directory.

For example, we can add a test to check if the `<Page />` component successfully renders a heading:

```ts
import Link from 'next/link'
 
export default function Home() {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">About</Link>
    </div>
  )
}
```

```ts
// __tests__/page.test.jsx
import '@testing-library/jest-dom'
import { render, screen } from '@testing-library/react'
import Page from '../app/page'
 
describe('Page', () => {
  it('renders a heading', () => {
    render(<Page />)
 
    const heading = screen.getByRole('heading', { level: 1 })
 
    expect(heading).toBeInTheDocument()
  })
})
```

Optionally, add a [snapshot test](https://jestjs.io/docs/snapshot-testing) to keep track of any unexpected changes in your component:

```js
// __tests__/snapshot.js
import { render } from '@testing-library/react'
import Page from '../app/page'
 
it('renders homepage unchanged', () => {
  const { container } = render(<Page />)
  expect(container).toMatchSnapshot()
})
```