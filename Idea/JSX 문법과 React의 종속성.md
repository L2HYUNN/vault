JSX가 jsxImportSource와 Babel의 runtime: "automatic" 옵션을 통해 React에 종속되지 않게 되었지만, 완전히 React에서 독립되었다고 보기는 어렵습니다. 다만, 이는 큰 진전으로, JSX 문법 자체가 특정 라이브러리(React)에 의존하지 않고 다양한 프레임워크나 라이브러리와 통합될 수 있는 길을 열어줍니다.

  

**이유와 설명**

  

**1. JSX와 React의 초기 의존성**

• JSX는 본래 React를 위해 만들어진 문법이었습니다. React.createElement를 호출하는 방식으로 동작했기 때문에 React에 강하게 결합되어 있었습니다.

• 개발자는 JSX 문법을 사용하려면 React를 반드시 import해야 했습니다.

  

예시 (기존 방식):

  

import React from 'react';

  

const element = <div>Hello, world!</div>;

  

이 코드는 Babel을 통해 아래처럼 변환되었습니다:

  

const element = React.createElement('div', null, 'Hello, world!');

  

**2. runtime: "automatic" 도입**

  

Babel의 runtime: "automatic" 옵션과 jsxImportSource 설정을 통해 React의 종속성을 없앨 수 있게 되었습니다.

  

예시 (자동 런타임):

  

const element = <div>Hello, world!</div>;

  

자동 런타임을 사용하면 Babel은 아래처럼 변환합니다:

  

import { jsx as _jsx } from "react/jsx-runtime";

  

const element = _jsx('div', { children: 'Hello, world!' });

  

이로 인해:

• JSX를 처리하는 로직이 React 내부의 jsx-runtime으로 옮겨졌습니다.

• 개발자는 React를 import할 필요가 없어졌습니다.

• JSX 자체는 특정 런타임(react/jsx-runtime)만 제공되면 동작할 수 있게 되었습니다.

  

**3. JSX의 React 독립 가능성**

  

JSX는 이제 **런타임(runtime)**만 제공되면 동작할 수 있기 때문에 React 외의 라이브러리나 사용자 정의 런타임에서도 사용할 수 있습니다. 즉, JSX는 특정 라이브러리와의 결합이 느슨해졌습니다.

  

예시: 사용자 정의 런타임

  

{

  "presets": [

    [

      "@babel/preset-react",

      {

        "runtime": "automatic",

        "importSource": "./my-jsx-runtime"

      }

    ]

  ]

}

  

my-jsx-runtime 파일에 JSX를 처리하는 jsx, jsxs, Fragment 함수만 정의하면 JSX를 독립적으로 사용할 수 있습니다.

  

**완전한 독립이 아니라는 이유**

1. **JSX는 런타임에 의존**

• JSX 자체는 문법이지만, 이를 실행 가능한 코드로 변환하려면 런타임(jsx, jsxs, Fragment 등)을 제공해야 합니다.

• 현재 런타임 구현은 React(또는 React와 호환되는 라이브러리)에 의존하거나 이를 직접 구현해야 합니다.

2. **React 생태계에서 파생된 표준**

• Babel의 JSX 트랜스파일링 방식과 runtime: "automatic"은 React 생태계에서 출발했습니다.

• 다른 프레임워크(예: Vue, Svelte)는 자체적인 문법이나 방식으로 JSX와 유사한 역할을 수행하기 때문에 JSX를 사용할 필요가 없습니다.

3. **JSX는 ECMAScript 표준이 아님**

• JSX는 JavaScript의 공식 표준이 아니며, React 커뮤니티와 Babel 같은 도구에서 관리되는 문법입니다. 표준화되지 않은 만큼 독립성을 논의하기엔 제한적입니다.

  

**결론**

  

JSX는 runtime: "automatic"과 jsxImportSource 옵션 덕분에 React에 대한 종속성이 줄어들었고, 사용자 정의 런타임을 통해 다양한 환경에서 사용할 수 있게 되었습니다. 이는 JSX가 React에 “독립적”인 문법처럼 보이게 하지만, 여전히 런타임 의존성을 가지므로 완전히 독립적이라고 보기는 어렵습니다.

  

**Q1. JSX를 표준화된 문법으로 발전시키려면 어떤 과정이 필요할까요?**

  

**Q2. React 외에 JSX를 활용할 수 있는 프레임워크나 라이브러리의 예시는 무엇이 있을까요?**

  

**Q3. JSX를 완전히 독립적으로 사용하려면 런타임 구현에서 어떤 추가 기능을 고려해야 할까요?**