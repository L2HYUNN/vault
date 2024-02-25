![[jsdom.webp]]

[[Jest Test Environment에 대하여]] 에서  `node`와 `jsdom`에 대하여 비교하면서 `jsdom`을 이용하는 것이 많은 퍼포먼스 오버헤드를 발생시킨다는 것을 알게 되었다. 이후 이러한 오버헤드를 직접 측정할 수 있다면 좋겠다는 생각에 방법을 찾던 중, **[JSDom benchmark](https://github.com/jsdom/jsdom/tree/main/benchmark)** 를 찾게 되었고 이를 통해 개인 환경에서 오버헤드를 살펴볼 수 있게 되었다.

## JSDom
[JSdom](https://github.com/jsdom/jsdom)은 Node 환경에서 DOM과 HTML 표준과 같은 수 많은 웹 표준을 순수 JavaScript로 구현한 것이다. `JSdom`을 이용하면 Node 환경에서 실제 브라우저에서 실행하는 것과 같이 JavaScript 코드를 동작시킬 수 있다.
### Benchmark

![[jsdom-benchmark-library.webp|600]]

`jsdom`에서는 위와 같이 자체적으로 벤치마크를 제공하고 있다. [jsdom/benchmark](https://github.com/jsdom/jsdom/tree/main/benchmark)에 들어가면 `dom`, `html`, `selecotrs`등 다양한 벤치마크를 확인할 수 있으며 스크립트 실행을 통해 손쉽게 벤치마크 테스트를 진행할 수 있다.

> [!info]
> `jsdom`에서는 벤치마크 측정을 위해 [benchmark.js](https://github.com/bestiejs/benchmark.js)를 사용하고 있다.

벤치마크 결과를 보기에 앞서 어떻게 벤치마크가 측정되는지 살펴보기 위해 해당 코드를 살펴보았다.

```js
// jsdom/benchmark/runner.js
let suitesToRun;
if (argv.suites) {
  suitesToRun = pathToSuites(benchmarks, argv.suites);
} else {
  suitesToRun = pathToSuites(benchmarks);
}

suitesToRun.forEach(consoleReporter);
```

먼저 벤치마크 `runner`인 [benchmark/runner.js](https://github.com/jsdom/jsdom/blob/main/benchmark/runner.js)를 살펴보면 `pathToSuites` 함수를 통해 실행할 `suites`들이 결정되고 각각의 suite에 [consoleReporter](https://github.com/jsdom/jsdom/blob/main/benchmark/console-reporter.js) 함수를 적용한다는 것을 알 수 있다. 

> [!info]
> [Software suite](https://en.wikipedia.org/wiki/Software_suite)는 관련 기능을 가진 컴퓨터 프로그램의 모음을 의미한다.

```js
function runNext() {
  /* eslint-disable no-invalid-this */
  if (this && this.off) {
    // there is no .once()
    this.off("complete", runNext);
  }
  /* eslint-enable no-invalid-this */

  const suite = suitesToRun.shift();
  if (!suite) {
    console.log("Done!");
    return;
  }

  suite.off("complete", runNext);
  suite.on("complete", runNext);
  suite.run({ async: true });
}

runNext();
```

이후 실행되는 `runNext` 함수는 `suites`들을 비동기로 실행한다.

`dom`, `html`, `jsdom`, `selectors` 폴더 에는 각각의 벤치마크 테스트를 위한 `suite`들이 위치해있다.

아래의 코드는 JSDOM 생성을 테스트하는 벤치마크이다.

```js
"use strict";
const Benchmark = require("benchmark");
const jsdomBenchmark = require("../jsdom-benchmark");
const { JSDOM } = require("../..");

exports["new JSDOM() defaults"] = function () {
  const suite = new Benchmark.Suite();

  suite.push(jsdomBenchmark({
    fn() {
      // eslint-disable-next-line no-new
      new JSDOM();
    }
  }));

  return suite;
};

exports["new JSDOM() with many elements"] = function () {
  let html = `<!doctype html><html><head><meta charset="UTF-8"><title></title></head><body>\n\n`;
  for (let i = 0; i < 1000; ++i) {
    html += `<a href='${i}.html'>${i}</a>\n`;
  }
  html += `\n</body></html>\n`;

  const suite = new Benchmark.Suite();

  suite.push(jsdomBenchmark({
    fn() {
      // eslint-disable-next-line no-new
      new JSDOM(html);
    }
  }));

  return suite;
};
```
