### Webpack이란?

Webpack은 최신 웹 애플리케이션에서 사용하는 모듈 번들러입니다. 웹 애플리케이션에서 사용하는 여러 자산(예: JavaScript, CSS, 이미지 등)을 하나 또는 여러 개의 번들 파일로 묶어줍니다. 이를 통해 코드의 관리와 배포가 쉬워지고, 웹 애플리케이션의 성능을 최적화할 수 있습니다.

- 모듈 번들링(Module Bundling): 진입점에 연결된 파일을 단일 파일로 묶어줍니다.
- 번들 최적화(Automatic Bundle Optimization): 번들 최적화를 통해, 보다 더 작은 번들을 생성하고 그만큼 빠르게 로딩할 수 있습니다.
- 코드 스플리팅(Code Splitting): 모듈을 청크(chunk)로 분리하여, 동적으로 필요한 모듈만 로딩할 수 있습니다.
- 트리 쉐이킹(Tree Shaking): 사용되지 않는 코드를 제거해 번들의 크기를 줄이고 성능을 향상시킬 수 있습니다.
- 개발 서버 실행(Development Server): 코드가 변경될 때마다 브라우저에 반영(HMR)되는 개발용 서버를 실행할 수 있습니다.

### 주요 기능

#### 1. 모듈 번들링(Module Bundling)

Webpack의 기본 기능은 다양한 모듈(파일)을 하나 또는 여러 개의 번들 파일로 묶는 것입니다. 이를 통해 애플리케이션의 모든 종속성을 한 번에 처리하고, 단일 파일로 묶어서 로딩 시간을 줄일 수 있습니다.

- **Entry**: Webpack이 모듈 그래프를 만들기 시작하는 진입점입니다. 일반적으로 애플리케이션의 메인 파일을 지정합니다.
  ```javascript
  module.exports = {
    entry: './src/index.js',
  };
  ```

- **Output**: 번들 파일의 출력 설정입니다. 번들 파일의 이름과 저장 위치를 정의합니다.
  ```javascript
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
  };
  ```

#### 2. 로더(Loaders)

로더는 Webpack이 JavaScript 외의 파일(CSS, 이미지, 폰트 등)을 처리할 수 있도록 도와줍니다. 파일의 내용을 읽고, 필요한 변환을 수행하여 번들링할 수 있게 해줍니다.

- **CSS 로더**: CSS 파일을 JavaScript 모듈로 변환합니다.
  ```javascript
  module.exports = {
    module: {
      rules: [
        {
          test: /\.css$/,
          use: ['style-loader', 'css-loader'],
        },
      ],
    },
  };
  ```

- **Babel 로더**: 최신 JavaScript 문법을 구형 브라우저에서도 호환되도록 트랜스파일링합니다.
  ```javascript
  module.exports = {
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env'],
            },
          },
        },
      ],
    },
  };
  ```

#### 3. 플러그인(Plugins)

플러그인은 로더가 할 수 없는 복잡한 작업을 수행할 수 있습니다. 번들 최적화, 자산 관리, 환경 변수 설정 등을 처리합니다.

- **HTMLWebpackPlugin**: HTML 파일을 자동으로 생성하고, 번들 파일을 포함시킵니다.
  ```javascript
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
      }),
    ],
  };
  ```

- **CleanWebpackPlugin**: 번들링 전에 출력 디렉토리를 정리합니다.
  ```javascript
  const { CleanWebpackPlugin } = require('clean-webpack-plugin');

  module.exports = {
    plugins: [
      new CleanWebpackPlugin(),
    ],
  };
  ```

#### 4. 코드 스플리팅(Code Splitting)

코드 스플리팅은 애플리케이션의 코드를 분할하여 필요한 코드만 로드하도록 합니다. 이를 통해 초기 로딩 속도를 개선하고, 사용자 경험을 향상시킬 수 있습니다.

- **동적 임포트**: 코드가 필요할 때만 로드합니다.
  ```javascript
  function loadComponent() {
    import('./component').then((Component) => {
      // Do something with the component
    });
  }
  ```

- **Vendor Chunk**: 외부 라이브러리(예: React, Lodash 등)를 별도의 파일로 분리합니다.
  ```javascript
  module.exports = {
    optimization: {
      splitChunks: {
        chunks: 'all',
      },
    },
  };
  ```

#### 5. 트리 쉐이킹(Tree Shaking)

트리 쉐이킹은 사용하지 않는 코드를 제거하여 최종 번들의 크기를 줄이는 기술입니다. ES6 모듈을 사용하여 불필요한 코드를 제거합니다.

```javascript
module.exports = {
  mode: 'production',
  // Tree shaking is enabled by default in production mode
};
```

#### 6. 개발 서버(Dev Server)

Webpack Dev Server는 로컬 개발 환경에서 애플리케이션을 테스트할 수 있는 서버를 제공합니다. 핫 모듈 교체(Hot Module Replacement)를 통해 코드 변경 시 페이지를 새로고침하지 않고 업데이트합니다.

```javascript
module.exports = {
  devServer: {
    contentBase: './dist',
    hot: true,
  },
};
```

### 결론

Webpack은 모듈 번들링, 로더, 플러그인, 코드 스플리팅, 트리 쉐이킹 등의 기능을 통해 복잡한 웹 애플리케이션을 효율적으로 관리하고 최적화할 수 있도록 도와줍니다. 이를 통해 개발 생산성을 높이고, 사용자 경험을 개선할 수 있습니다.

**Q1:** Webpack 설정 파일에서 자주 사용되는 플러그인과 그 역할은 무엇인가요?

**Q2:** 코드 스플리팅을 통해 애플리케이션의 성능을 어떻게 개선할 수 있나요?

**Q3:** Webpack Dev Server를 사용하여 로컬 개발 환경을 설정하는 방법에 대해 설명해 주세요.