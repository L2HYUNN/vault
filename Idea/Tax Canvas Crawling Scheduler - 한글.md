
# Tax Canvas Crawling Scheduler

Tax Canvas [국세법령정보시스템](https://taxlaw.nts.go.kr/index.do) 크롤러 및 스케쥴러

## 개요
Tax Canvas Crawling Scheduler는 아래의 메뉴들의 게시글 링크와 게시글 정보를 크롤링하고 이것을 스케쥴링할 수 있습니다:

1. 사전답변
2. 질의회신
3. 과세기준자문
4. 고시서면질서
5. 과세적부
6. 이의신청
7. 심사청구
8. 심판청구
9. 판례
10. 헌재

## 핵심 스택
### [Puppeteer](https://github.com/puppeteer/puppeteer?tab=readme-ov-file)
Puppeteer는 [DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) 또는 [WebDriver BiDi](https://pptr.dev/webdriver-bidi)를 통해 Chrome 또는 Firefox를 제어할 수 있는 고급 API를 제공하는 Node.js 라이브러리입니다. Puppeteer는 기본적으로 headless 모드(보이는 UI 없음)로 실행되지만, 보이는 ("headful") 브라우저로 실행되도록 구성할 수 있습니다.

### [Node Schedule](https://github.com/node-schedule/node-schedule)
Node Schedule은 Node.js용 유연한 cron과 유사한 스케쥴러입니다. 특정 날짜에 작업(임의 함수)을 실행하도록 스케쥴링할 수 있으며, 선택적 반복 규칙이 포함됩니다. 단일 타이머만 사용하여 향후 작업을 초/분마다 재평가하는 대신 실행합니다.

## 시스템 요구 사항

- Node 18+. Puppeteer는 최신 [maintenance LTS](https://github.com/nodejs/Release#release-schedule) 버전의 Node를 따릅니다.
    
- 운영 체제:
    - Windows, x64 아키텍처
    - MacOS, x64 및 arm64 아키텍처
    - Debian/Ubuntu Linux, x64 아키텍처
        - 필요한 시스템 패키지 [https://source.chromium.org/chromium/chromium/src/+/main:chrome/installer/linux/debian/dist_package_versions.json](https://source.chromium.org/chromium/chromium/src/+/main:chrome/installer/linux/debian/dist_package_versions.json)

- TypeScript 4.7.4+ (TypeScript를 사용할 경우)

## 설치

1. 리포지토리를 클론합니다:

```bash
git clone https://github.com/hamish-official/scheduler.git
```

2. 프로젝트 디렉토리로 이동합니다:

```bash
cd scheduler/codes
```

3. 필요한 패키지를 설치합니다:

```bash
npm install
```

## 시작하기

**크롤러 실행**

크롤러를 실행하려면 다음 명령을 실행합니다:

- 링크
국세법령정보시스템에서 상세 게시물로 연결되는 링크를 스크랩합니다.

```bash
npm run link
```

- 콘텐츠
국세법령정보시스템의 상세 게시물에서 콘텐츠를 스크랩합니다.

```bash
npm run content
```

스크랩된 콘텐츠는 `jsons/[taxLawMenuName]` 폴더에 저장됩니다.

> [!warning]
> 각 tax law 스크랩 함수를 실행하기 위해서는 `src` 폴더에 각 메뉴의 이름에 맞는 `[taxLawMenuName].txt` 링크 파일이 존재해야 합니다.
> 
>  **예: `과세적부.js`를 실행하기 위해서는 `src/과세적부.txt` 파일이 존재해야 합니다.**
>  
> 이를 해결할 수 있는 가장 쉬운 방법은 `npm run link`를 실행하는 것입니다.

**크롤러 스케쥴링**

스케쥴러를 설정하고 실행하려면 다음 명령을 사용합니다:

```bash
npm run start
```

스케쥴러는 기본적으로 매주 월요일 12:30 PM에 실행되도록 구성되어 있습니다.

## 커스터마이징

### 링크 크롤링

링크 스크랩을 커스터마이징하려면 아래 파일을 수정하십시오:

```js
// src/scrapeLink.js
const { 링크_스크랩_동기_100개 } = require("./링크");

const scrape = async () => {
  await 링크_스크랩_동기_100개();

  return "Link scraping completed";
};

scrape();
```

#### 옵션

- **링크_스크랩**: 메뉴에 존재하는 모든 게시글에 대한 링크를 비동기로 스크랩합니다.
- **링크_스크랩_동기**: 메뉴에 존재하는 모든 게시글에 대한 링크를 동기로 스크랩합니다.
- **링크_스크랩_동기_100개 (default)**: 메뉴에 존재하는 게시글을 최신순으로 100개 동기로 스크랩합니다.

*참고: 다른 수의 링크를 스크랩하고 싶다면 `링크.js` 파일에 새로운 링크 스크랩 함수를 추가할 수 있습니다.*

### 콘텐츠 크롤링

콘텐츠 스크랩을 커스터마이징하려면 아래 파일을 수정하십시오:

```js
// src/scrapeContents.js
const 사전답변_스크랩 = require("./사전답변");
const 질의회신_스크랩 = require("./질의회신");
const 과세기준자문_스크랩 = require("./과세기준자문");
const 고시서면질서_스크랩 = require("./고시서면질의");
const 과세적부_스크랩 = require("./과세적부");
const 이의신청_스크랩 = require("./이의신청");
const 심사청구_스크랩 = require("./심사청구");
const 심판청구_스크랩 = require("./심판청구");
const 판례_스크랩 = require("./판례");
const 헌재_스크랩 = require("./헌재");

const scrape = async () => {
  await 사전답변_스크랩();
  await 질의회신_스크랩();
  await 과세기준자문_스크랩();
  await 고시서면질서_스크랩();
  await 과세적부_스크랩();
  await 이의신청_스크랩();
  await 심사청구_스크랩();
  await 심판청구_스크랩();
  await 판례_스크랩();
  await 헌재_스크랩();

  return "Content scraping completed";
};

scrape();
```

각 콘텐츠 스크랩 함수는 하나의 메뉴에 존재하는 게시글에 대한 스크랩을 가능하게 합니다. 이러한 함수를 추가하고 제거함으로써 콘텐츠를 스크랩할 함수를 결정할 수 있습니다.

### 스케쥴러
스케쥴러는 [node-schedule](https://github.com/node-schedule/node-schedule)을 사용하고 있기 때문에 제공되는 아래의 옵션을 이용해 스케쥴링 시간을 결정할 수 있습니다:

### Cron 스타일 스케쥴링

cron 형식은 다음과 같이 구성됩니다:
```
*    *    *    *    *    *
┬    ┬    ┬    ┬    ┬    ┬
│    │    │    │    │    │
│    │    │    │    │    └ 요일 (0 - 7) (0 또는 7은 일요일)
│    │    │    │    └───── 월 (1 - 12)
│    │    │    └────────── 일 (1 - 31)
│    │    └─────────────── 시간 (0 - 23)
│    └──────────────────── 분 (0 - 59)
└───────────────────────── 초 (0 - 59, 선택 사항)
```

cron 형식의 예:

```js
const schedule = require('node-schedule');

const job = schedule.scheduleJob('42 * * * *', function(){
  console.log('The answer to life, the universe, and everything!');
});
```

이 코드는 매 시간 42분에 작업을 실행합니다 (예: 19:42, 20:42 등).

그리고:

```js
const job = schedule.scheduleJob('0 17 ? * 0,4-6', function(){
  console.log('Today is recognized by Rebecca Black!');
});
```

이 코드는 매주 일요일과 목요일부터 토요일까지 오후 5시에 작업을 실행합니다.

또한, 작업이 실행될 때마다 실행 예정 시간을 확인할 수도 있습니다:
```js
const job = schedule.scheduleJob('0 1 * * *', function(fireDate){
  console.log('This job was supposed to run at ' + fireDate + ', but actually ran at ' + new Date());
});
```
이 기능은 시스템이 바쁠 때 작업 실행의 지연 여부를 확인하거나, 감사 목적으로 모든 작업 실행 기록을 저장하는 데 유용합니다.

#### 지원되지 않는 Cron 기능

현재 `W` (가장 가까운 평일) 및 `L` (월/주의 마지막 날)은 지원되지 않습니다. 
대부분의 다른 인기 있는 cron 구현에서 지원하는 기능은 잘 작동할 것입니다.
`#` (월의 n번째 평일) 포함됩니다.

[cron-parser]를 사용하여 crontab 지시사항을 구문 분석합니다.

### 날짜 기반 스케쥴링

특정 날짜와 시간에 작업을 실행하려면 다음과 같이 합니다. (예: 2012년 12월 21일 오전 5:30에 작업 실행).

```js
const schedule = require('node-schedule');
const date = new Date(2012, 11, 21, 5, 30, 0);

const job = schedule.scheduleJob(date, function(){
  console.log('The world is going to end today.');
});
```

미래에 현재 데이터를 사용하려면 바인딩을 사용할 수 있습니다:

```js
const schedule = require('node-schedule');
const date = new Date(2012, 11, 21, 5, 30, 0);
const x = 'Tada!';
const job = schedule.scheduleJob(date, function(y){
  console.log(y);
}.bind(null,x));
x = 'Changing Data';
```
이 코드는 예약된 작업이 실행될 때 'Tada!'를 로그로 기록합니다, 'Changing Data'가 아닌.

### 반복 규칙 스케쥴링

작업이 반복될 시점을 지정하기 위해 반복 규칙을 만들 수 있습니다. 예를 들어, 매 시간 42분에 함수를 실행하는 규칙을 다음과 같이 작성할 수 있습니다:

```js
const schedule = require('node-schedule');

const rule = new schedule.RecurrenceRule();
rule.minute = 42;

const job = schedule.scheduleJob(rule, function(){
  console.log('The answer to life, the universe, and everything!');
});
```

배열을 사용하여 허용 가능한 값의 목록을 지정하고, `Range` 객체를 사용하여 시작 및 종료 값을 지정할 수 있습니다. 예를 들어, 목요일, 금요일, 토요일, 일요일 오후 5시에 메시지를 출력하려면 다음과 같이 합니다:

```js
const rule = new schedule.RecurrenceRule();
rule.dayOfWeek = [0, new schedule.Range(4, 6)];
rule.hour = 17;
rule.minute = 0;

const job = schedule.scheduleJob(rule, function(){
  console.log('Today is recognized by Rebecca Black!');
});
```

타임존도 지원됩니다. 다음은 UTC 타임존에서 매일 시작할 때 실행되는 예입니다:

```js
const rule = new schedule.RecurrenceRule();
rule.hour = 0;
rule.minute = 0;
rule.tz = 'Etc/UTC';

const job = schedule.scheduleJob(rule, function(){
  console.log('A new day has begun in the UTC timezone!');
});
```

허용 가능한 tz(타임존) 값의 목록은 <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones>에서 찾을 수 있습니다.

#### RecurrenceRule 속성

- `second (0-59)`
- `minute (0-59)`
- `hour  (0-23)`
- `date  (1-31)`
- `month (0-11)`
- `year`
- `dayOfWeek (0-6) 일요일부터 시작`
- `tz`

> **참고**: 반복 규칙의 기본값은 `null`입니다 (cron과의 친숙함을 위해 초는 0입니다). 위에서 `minute`을 0으로 명시적으로 설정하지 않았다면, 메시지가 오후 5시, 5시 1분, 5시 2분, ..., 5시 59분에 로그로 기록됩니다. 아마 원하지 않는 결과일 것입니다.

#### 객체 리터럴 구문

조금 더 쉽게 하기 위해 객체 리터럴 구문도 지원됩니다. 예를 들어, 매주 일요일 오후 2시 30분에 메시지를 출력하려면 다음과 같이 합니다:

```js
const job = schedule.scheduleJob({hour: 14, minute: 30, dayOfWeek: 0}, function(){
  console.log('Time for tea!');
});
```

#### 시작 시간 및 종료 시간 설정

이 예제에서는 5초 후에 실행되고 10초 후에 중지됩니다. 위의 규칙 데이터가 이를 지원합니다.

```js
const startTime = new Date(Date.now() + 5000);
const endTime = new Date(startTime.getTime() + 5000);
const job = schedule.scheduleJob({ start: startTime, end: endTime, rule: '*/1 * * * * *' }, function(){
  console.log('Time for tea!');
});
```

## 참고자료

• [Puppeteer Documentation](https://github.com/puppeteer/puppeteer)

• [Node Schedule Documentation](https://github.com/node-schedule/node-schedule)