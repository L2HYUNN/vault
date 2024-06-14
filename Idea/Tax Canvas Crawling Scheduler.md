Tax Canvas [National Tax Law Information System](https://taxlaw.nts.go.kr/index.do) Crawler and Scheduler

## Overview
The Tax Canvas Crawling Scheduler can crawl and schedule the following contents:

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

## Core Stack
### [puppeteer](https://github.com/puppeteer/puppeteer?tab=readme-ov-file)
Puppeteer is a Node.js library which provides a high-level API to control Chrome or Firefox over the [DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) or [WebDriver BiDi](https://pptr.dev/webdriver-bidi). Puppeteer runs in the headless (no visible UI) by default but can be configured to run in a visible ("headful") browser.

### [node-shedule](https://github.com/node-schedule/node-schedule)
Node Schedule is a flexible cron-like and not-cron-like job scheduler for Node.js. It allows you to schedule jobs (arbitrary functions) for execution at specific dates, with optional recurrence rules. It only uses a single timer at any given time (rather than reevaluating upcoming jobs every second/minute).

## System Requirements

- Node 18+. Puppeteer follows the latest [maintenance LTS](https://github.com/nodejs/Release#release-schedule) version of Node
    
- Operating systems:
    
    - Windows, x64 architecture
    - MacOS, x64 and arm64 architectures
    - Debian/Ubuntu Linux, with x64 architecture
        - Required system packages [https://source.chromium.org/chromium/chromium/src/+/main:chrome/installer/linux/debian/dist_package_versions.json](https://source.chromium.org/chromium/chromium/src/+/main:chrome/installer/linux/debian/dist_package_versions.json)

- TypeScript 4.7.4+ (If used with TypeScript)

## Installation

1. Clone the repository:    

```bash
git clone https://github.com/hamish-official/scheduler.git
```

2. Navigate to the project directory:

```bash
cd scheduler/codes
```

3. Install the required packages:

```bash
npm install
```

## Getting started

**Running the Crawler**
To run the crawler, execute the following command:

- Link
Scrapes links to detailed posts from the National Tax Law Information System.

```bash
npm run link
```

- Content
Scrapes content from detailed posts in the National Tax Law Information System.

```bash
npm run content
```

> [!warning]
> To execute each tax law scraping function, a `[taxLawMenuName].txt` link file must exist in the `src` folder matching the name of each menu.
> 
>  **For example, to run `과세적부.js`, the file `src/과세적부.txt` must be present.**
>  
>The easiest way to resolve this is to run `npm run link`.

**Scheduling the Crawler**
To set up and run the scheduler, use the following command:

```bash
npm run start
```

## Customization

### Crawling Links

You can customize link scraping by modifying the file below:

```js
// src/scrapeLink.js
const { 링크_스크랩_동기_100개 } = require("./링크");

const scrape = async () => {
  await 링크_스크랩_동기_100개();

  return "Link scraping completed";
};

scrape();
```

#### Options

- **링크_스크랩**: Asynchronously scrapes links for all posts in the menu.
- **링크_스크랩_동기**: Synchronously scrapes links for all posts in the menu.
- **링크_스크랩_동기_100개 (default)**: Synchronously scrapes the latest 100 posts in the menu.

*Note: If you want to scrape a different number of links, you can add a new link scraping function in the `링크.js` file.*

### Crawling Content

You can customize content scraping by modifying the file below:

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

Each content scraping function allows you to scrape contents from a specific menu. You can decide which content to scrape by adding or removing these functions.

### Scheduler





**Appendix**

## References

• [Puppeteer Documentation](https://github.com/puppeteer/puppeteer)

• [Node Schedule Documentation](https://github.com/node-schedule/node-schedule)