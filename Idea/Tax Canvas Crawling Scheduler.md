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

The scraped content is saved in the `jsons/[taxLawMenuName]` folder.

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

The scheduler is configured by default to run every Monday at 12:30 PM.

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
The scheduler uses [node-schedule](https://github.com/node-schedule/node-schedule), allowing you to determine the scheduling time using the provided options below:

### Cron-style Scheduling

The cron format consists of:
```
*    *    *    *    *    *
┬    ┬    ┬    ┬    ┬    ┬
│    │    │    │    │    │
│    │    │    │    │    └ day of week (0 - 7) (0 or 7 is Sun)
│    │    │    │    └───── month (1 - 12)
│    │    │    └────────── day of month (1 - 31)
│    │    └─────────────── hour (0 - 23)
│    └──────────────────── minute (0 - 59)
└───────────────────────── second (0 - 59, OPTIONAL)
```

Examples with the cron format:

```js
const schedule = require('node-schedule');

const job = schedule.scheduleJob('42 * * * *', function(){
  console.log('The answer to life, the universe, and everything!');
});
```

Execute a cron job when the minute is 42 (e.g. 19:42, 20:42, etc.).

And:

```js
const job = schedule.scheduleJob('0 17 ? * 0,4-6', function(){
  console.log('Today is recognized by Rebecca Black!');
});
```

Execute a cron job every 5 Minutes = */5 * * * *

You can also get when it is scheduled to run for every invocation of the job:
```js
const job = schedule.scheduleJob('0 1 * * *', function(fireDate){
  console.log('This job was supposed to run at ' + fireDate + ', but actually ran at ' + new Date());
});
```
This is useful when you need to check if there is a delay of the job invocation when the system is busy, or save a record of all invocations of a job for audit purpose.
#### Unsupported Cron Features

Currently, `W` (nearest weekday) and `L` (last day of month/week) are not supported. 
Most other features supported by popular cron implementations should work just fine, 
including `#` (nth weekday of the month).

[cron-parser] is used to parse crontab instructions.

### Date-based Scheduling

Say you very specifically want a function to execute at 5:30am on December 21, 2012.
Remember - in JavaScript - 0 - January, 11 - December.

```js
const schedule = require('node-schedule');
const date = new Date(2012, 11, 21, 5, 30, 0);

const job = schedule.scheduleJob(date, function(){
  console.log('The world is going to end today.');
});
```

To use current data in the future you can use binding:

```js
const schedule = require('node-schedule');
const date = new Date(2012, 11, 21, 5, 30, 0);
const x = 'Tada!';
const job = schedule.scheduleJob(date, function(y){
  console.log(y);
}.bind(null,x));
x = 'Changing Data';
```
This will log 'Tada!' when the scheduled Job runs, rather than 'Changing Data',
which x changes to immediately after scheduling.

### Recurrence Rule Scheduling

You can build recurrence rules to specify when a job should recur. For instance,
consider this rule, which executes the function every hour at 42 minutes after the hour:

```js
const schedule = require('node-schedule');

const rule = new schedule.RecurrenceRule();
rule.minute = 42;

const job = schedule.scheduleJob(rule, function(){
  console.log('The answer to life, the universe, and everything!');
});
```

You can also use arrays to specify a list of acceptable values, and the `Range`
object to specify a range of start and end values, with an optional step parameter.
For instance, this will print a message on Thursday, Friday, Saturday, and Sunday at 5pm:

```js
const rule = new schedule.RecurrenceRule();
rule.dayOfWeek = [0, new schedule.Range(4, 6)];
rule.hour = 17;
rule.minute = 0;

const job = schedule.scheduleJob(rule, function(){
  console.log('Today is recognized by Rebecca Black!');
});
```

Timezones are also supported. Here is an example of executing at the start of every day in the UTC timezone.

```js
const rule = new schedule.RecurrenceRule();
rule.hour = 0;
rule.minute = 0;
rule.tz = 'Etc/UTC';

const job = schedule.scheduleJob(rule, function(){
  console.log('A new day has begun in the UTC timezone!');
});
```

A list of acceptable tz (timezone) values can be found at <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones>

#### RecurrenceRule properties

- `second (0-59)`
- `minute (0-59)`
- `hour  (0-23)`
- `date  (1-31)`
- `month (0-11)`
- `year`
- `dayOfWeek (0-6) Starting with Sunday`
- `tz`


> **Note**: It's worth noting that the default value of a component of a recurrence rule is
> `null` (except for second, which is 0 for familiarity with cron). *If we did not
> explicitly set `minute` to 0 above, the message would have instead been logged at
> 5:00pm, 5:01pm, 5:02pm, ..., 5:59pm.* Probably not what you want.

#### Object Literal Syntax

To make things a little easier, an object literal syntax is also supported, like
in this example which will log a message every Sunday at 2:30pm:

```js
const job = schedule.scheduleJob({hour: 14, minute: 30, dayOfWeek: 0}, function(){
  console.log('Time for tea!');
});
```

#### Set StartTime and EndTime

It will run after 5 seconds and stop after 10 seconds in this example.
The ruledat supports the above.

```js
const startTime = new Date(Date.now() + 5000);
const endTime = new Date(startTime.getTime() + 5000);
const job = schedule.scheduleJob({ start: startTime, end: endTime, rule: '*/1 * * * * *' }, function(){
  console.log('Time for tea!');
});
```

## References

• [Puppeteer Documentation](https://github.com/puppeteer/puppeteer)

• [Node Schedule Documentation](https://github.com/node-schedule/node-schedule)