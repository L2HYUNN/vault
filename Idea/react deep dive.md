## 1주차 / 개발 환경 구축과 JSX 이해하기

### Vite
Vite는 차세대 **빌드 도구**로, 빠른 개발 환경과 간결한 설정을 제공한다.
**빠른 빌드 시간**과 **간단한 설정**을 목표로 만들어졌다.

#### 주요 특징

1. **개발 서버**
	- 브라우저의 네이티브 ES 모듈 기능을 활용하여 번들링 없이 JavaScript 모듈을 로드한다.
	- 변경 사항이 발생하면 필요한 부분만 다시 로드하는 방식(HMR)을 사용한다.

2. **빠른 빌드**
	- Vite는 ESBuild를 사용하여 코드를 사전 번들링(pre-bundling)하므로 초기 빌드 속도가 매우 빠르다.
	- ESBuild는 Go 언어로 작성되어 있으며 Webpack, Rollup보다 훨씬 빠르다.

3. **간단한 설정**
	- Vite는 기본적으로 최소한의 설정만 요구한다.
	- 다양한 기본 템플릿을 지원한다.


### @vitejs/plugin-react


## Bundler
### 번들러 란?

간단하게 설명하자면 **여러개의 파일들을 하나의 파일로 묶어주는 도구**

[차세대 번들러 비교 및 분석 (feat. webpack, rollup, esbuild, vite)](https://bepyan.github.io/blog/2023/bundlers)
[번들러와 빌드 도구의 이해](https://www.heropy.dev/p/x8iedW)


### esbuild

#### JSX Import source
[[JSX 문법과 React의 종속성 ]]


### tsconfig

#### tsconfig 생성 

```zsh
tsc --init
```

#### tsconfig 전역 속성

```json
{
    // TypeScript 컴파일러의 옵션들을 지정하는 속성
    "compilerOptions": { 
        "target": "es5", 
        "module": "commonjs", 
        "strict": true, 
        "sourceMap": true
        // ... 무수히 많은 속성들
    },

    // 컴파일할 파일들의 개별 목록을 지정하는 속성
    "files": ["src/main.ts", "src/utils.ts"],

    // 컴파일할 파일들을 지정하는 속성 (와일드 카드 패턴으로 묶어 표현) 
    "include": [ "src/**/*.ts" ], 
    
    // 컴파일 대상에서 제외할 파일들을 지정하는 속성 
    "exclude": [ "node_modules", "**/*.test.ts" ], 
    
    // 다른 tsconfig.json 파일을 상속받아서 설정을 재사용할 수 있게 해주는 속성 
    "extends": "./configs/base.json", 
    
    // 여러 개의 하위 프로젝트로 구성된 프로젝트의 의존 관계를 지정하는 속성 
    "references": [ 
        { "path": "./subproject1" }, 
        { "path": "./subproject2" } 
    ], 
    
    // 타입 습득(type acquisition)과 관련된 옵션들을 지정하는 속성 
    "typeAcquisition": { 
        "enable": true, 
        "include": ["jquery"], 
        "exclude": ["react"] 
    }, 
        
    // watch 모드와 관련된 옵션들을 지정하는 속성 
    "watchOptions": { 
        "watchFile": "useFsEvents", 
        "watchDirectory": "useFsEvents", 
        "fallbackPolling": "dynamicPriority"
    }
}
```

#### compilerOptions

```json
{
  "compilerOptions": {
  
    /* 기본 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "incremental": true,                   /* 증분 컴파일 활성화 */ 
    "target": "es5",                          /* ECMAScript 목표 버전 설정: 'ES3'(기본), 'ES5', 'ES2015', 'ES2016', 'ES2017','ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "esnext",                       /* 생성될 모듈 코드 설정: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    "lib": ["dom", "dom.iterable", "esnext"], /* 컴파일 과정에 사용될 라이브러리 파일 설정 */
    "allowJs": true,                          /* JavaScript 파일 컴파일 허용 */
    "checkJs": true,                       /* .js 파일 오류 리포트 설정 */
    "jsx": "react",                           /* 생성될 JSX 코드 설정: 'preserve', 'react-native', or 'react'. */
    "declaration": true,                   /* '.d.ts' 파일 생성 설정 */
    "declarationMap": true,                /* 해당하는 각 '.d.ts'파일에 대한 소스 맵 생성 */
    "sourceMap": true,                     /* 소스맵 '.map' 파일 생성 설정 */
    "outFile": "./",                       /* 복수 파일을 묶어 하나의 파일로 출력 설정 */
    "outDir": "./dist",                    /* 출력될 디렉토리 설정 */
    "rootDir": "./",                       /* 입력 파일들의 루트 디렉토리 설정. --outDir 옵션을 사용해 출력 디렉토리 설정이 가능 */
    "composite": true,                     /* 프로젝트 컴파일 활성화 */
    "tsBuildInfoFile": "./",               /* 증분 컴파일 정보를 저장할 파일 지정 */
    "removeComments": true,                /* 출력 시, 주석 제거 설정 */
    "noEmit": true,                           /* 출력 방출(emit) 유무 설정 */
    "importHelpers": true,                 /* 'tslib'로부터 헬퍼를 호출할 지 설정 */
    "downlevelIteration": true,            /* 'ES5' 혹은 'ES3' 타겟 설정 시 Iterables 'for-of', 'spread', 'destructuring' 완벽 지원 설정 */
    "isolatedModules": true,                  /* 각 파일을 별도 모듈로 변환 ('ts.transpileModule'과 유사) */

    /* 엄격한 유형 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "strict": true,                           /* 모든 엄격한 유형 검사 옵션 활성화 */
    "noImplicitAny": true,                 /* 명시적이지 않은 'any' 유형으로 표현식 및 선언 사용 시 오류 발생 */
    "strictNullChecks": true,              /* 엄격한 null 검사 사용 */
    "strictFunctionTypes": true,           /* 엄격한 함수 유형 검사 사용 */
    "strictBindCallApply": true,           /* 엄격한 'bind', 'call', 'apply' 함수 메서드 사용 */
    "strictPropertyInitialization": true,  /* 클래스에서 속성 초기화 엄격 검사 사용 */
    "noImplicitThis": true,                /* 명시적이지 않은 'any'유형으로 'this' 표현식 사용 시 오류 발생 */
    "alwaysStrict": true,                  /* 엄격모드에서 구문 분석 후, 각 소스 파일에 "use strict" 코드를 출력 */

    /* 추가 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "noUnusedLocals": true,                /* 사용되지 않은 로컬이 있을 경우, 오류로 보고 */
    "noUnusedParameters": true,            /* 사용되지 않은 매개변수가 있을 경우, 오류로 보고 */
    "noImplicitReturns": true,             /* 함수가 값을 반환하지 않을 경우, 오류로 보고 */
    "noFallthroughCasesInSwitch": true,       /* switch 문 오류 유형에 대한 오류 보고 */
 	"noUncheckedIndexedAccess": true,      /* 인덱스 시그니처 결과에 'undefined' 포함 */

    /* 모듈 분석 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "moduleResolution": "node",               /* 모듈 분석 방법 설정: 'node' (Node.js) 또는 'classic' (TypeScript pre-1.6). */
    "baseUrl": "./",                       /* 절대 경로 모듈이 아닌, 모듈이 기본적으로 위치한 디렉토리 설정 (예: './modules-name') */
    "paths": {},                           /* 'baseUrl'을 기준으로 상대 위치로 가져오기를 다시 매핑하는 항목 설정 */
    "rootDirs": [],                        /* 런타임 시 프로젝트 구조를 나타내는 로트 디렉토리 목록 */
    "typeRoots": [],                       /* 유형 정의를 포함할 디렉토리 목록 */
    "types": [],                           /* 컴파일 시 포함될 유형 선언 파일 입력 */
    "allowSyntheticDefaultImports": true,     /* 기본 출력(default export)이 없는 모듈로부터 기본 호출을 허용 (이 코드는 단지 유형 검사만 수행) */
    "esModuleInterop": true,                   /* 모든 가져오기에 대한 네임스페이스 객체 생성을 통해 CommonJS와 ES 모듈 간의 상호 운용성을 제공. 'allowSyntheticDefaultImports' 암시 */
    "preserveSymlinks": true,              /* symlinks 실제 경로로 결정하지 않음 */
    "allowUmdGlobalAccess": true,          /* 모듈에서 UMD 글로벌에 접근 허용 */

    /* 소스맵 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "sourceRoot": "./",                    /* 디버거(debugger)가 소스 위치 대신 TypeScript 파일을 찾을 위치 설정 */
    "mapRoot": "./",                       /* 디버거가 생성된 위치 대신 맵 파일을 찾을 위치 설정 */
    "inlineSourceMap": true,               /* 하나의 인라인 소스맵을 내보내도록 설정 */
    "inlineSources": true,                 /* 하나의 파일 안에 소스와 소스 코드를 함께 내보내도록 설정. '--inlineSourceMap' 또는 '--sourceMap' 설정이 필요 */

    /* 실험적인 기능 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "experimentalDecorators": true,        /* ES7 데코레이터(decorators) 실험 기능 지원 설정 */
    "emitDecoratorMetadata": true,         /* 데코레이터를 위한 유형 메타데이터 방출 실험 기능 지원 설정 */
    
    /* 고급 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "skipLibCheck": true,                     /* 선언 파일 유형 검사 스킵 */
    "forceConsistentCasingInFileNames": true  /* 동일한 파일에 대한 일관되지 않은 케이스 참조를 허용하지 않음 */
    
  }
}
```