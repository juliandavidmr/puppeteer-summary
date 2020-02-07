# Puppeteer's summary

## Overview

- Documentation and useful resources
    - [Puppeteer API docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md)
    - [Puppeteer documentation](https://pptr.dev)
    - [Awesome puppeteer](https://github.com/transitive-bullshit/awesome-puppeteer)
    - [Puppeteer Firefox](https://github.com/puppeteer/puppeteer/tree/master/experimental/puppeteer-firefox)
    - [Puppeteer + Cucumber :star2:](#cucumberpuppeteer)

## Puppeteer

### Common uses

- Scrape web pages
- Automate form submissions
- Perform any kind of browser automation
- Track page loading performance
- Create server-side rendered versions of single page apps
- Make screenshots
- Create automating testing
- Generate PDF from web pages

### Common functions

> Base: `const puppeteer = require('puppeteer')`

| Function                               | Description                                                  |
|----------------------------------------|--------------------------------------------------------------|
| `puppeteer.launch({ headless:false })` | Promise which resolves to browser instance. [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#puppeteerlaunchoptions)  |

#### `Browser`

> Base: `const browser = await puppeteer.launch()`

| Function                                     | Description                                                                 |
| -------------------------------------------- | --------------------------------------------------------------------------- |
| `browser.newPage()`                          | Create a new Page object. The Page is created in a default browser context. |
| `browser.createIncognitoBrowserContext()`    | Creates a new browser context.                                              |
| `browser.close()`                            | Closes the browser with the default context.                                |

#### `Page`

> Base: `const page = await browser.newPage();`

| Function                                           | Description |
|----------------------------------------------------|-------------|
| `page.setViewport({ width: 600, height: 400 })`    | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagesetviewportviewport)            |
| `page.goto('https://website.com')`                 | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagegotourl-options)            |
| `page.hover('button#exampleId')`                   | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagehoverselector) |
| `page.focus('input#name')`                         | Focuses on the selector passed as parameter. |
| `page.type('input#name', 'Example text')`          | Types into a selector that identifies a form element.  |
| `page.waitForSelector('body')`                     | Wait for the selector to appear in page. [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagewaitforselectorselector-options)    |
| `page.screenshot({path: 'screenshot.png'})`        | Promise which resolves to buffer or a base64 string (depending on the value of encoding) with captured screenshot. [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagescreenshotoptions) |
| `page.emulate(puppeteer.devices['iPhone X'])`      | Emulates given device metrics and user agent, see [list of devices here](https://github.com/GoogleChrome/puppeteer/blob/master/lib/DeviceDescriptors.js) |

### Examples

- Generate pdf:

```ts
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://github.com/');
  await page.pdf({ path: 'github.pdf', format: 'A4' });
  browser.close();
})();
```

- Take screenshot:

```ts
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.setViewport({ width: 767, height: 1024 });
  await page.goto('https://getbootstrap.com/');
  await page.screenshot({
    path: 'bootstrap.png',
    fullPage: true
  });
  browser.close();
})();
```

- Emulate device and take screenshot:

```ts
const fs = require('fs');
const puppeteer = require('puppeteer');

run().then(() => console.log('Done')).catch(error => console.log(error));

async function run() {
    const browser = await puppeteer.launch({ headless: false });
    const page = await browser.newPage();

    await page.emulate(puppeteer.devices['iPhone X']);

    await page.goto('https://www.google.com');

    const buf = await page.screenshot();
    fs.writeFileSync('./screenshot.png', buf);

    await browser.close();
}
```
<a name="cucumberpuppeteer"></a>
## Cucumber + Puppeteer

### Steps to create a new project of tests

#### 1) Create Basic Project

````bash
mkdir my-test-project && cd ./my-test-project && npm init -y
````

#### 2) Add basic dependencies

````bash
npm i chai cucumber cucumber-html-reporter cucumber-pretty puppeteer -S
````

#### 3) Open the project with the favorite editor and replace the next script commands in `package.json`

````json
"scripts": {
    "test:e2e": "node ./node_modules/cucumber/bin/cucumber-js -f node_modules/cucumber-pretty"  
}
````

#### 4) Now run the test and verify that there are no scenarios yet

````bash
npm run test:e2e
````

#### 5) Create the basic structure of features folder

````bash
mkdir .\features\steps && echo Feature: First feature > .\features\my_first.feature && echo // Step definition > .\features\steps\hello_world.step.js
````

#### 6) In .\features\my_first.feature file include the first scenario
````javascript
  Scenario: Test of my first scenario
    Given I run the first step using the text 'Hello W'
````
#### 7) Now run the test and verify that it fails

````bash
npm run test:e2e
````

#### 8) In .\features\steps\hello_world.step.js file include the first step definition

````javascript
const {Then} = require('cucumber');
const {expect} = require('chai');
Then('I run the first step using the text {string}', {timeout: 20 * 1000}, async (textValue) => {
    // console.log('textValue parameter', textValue);
    await expect(textValue).to.have.string('World');
});
````

#### 9) Now run the test and verify that it fails

````bash
npm run test:e2e
````

#### 10) In .\features\my_first.feature file fix the Step call

````javascript
    Given I run the first step using the text 'Hello world'
````

### Steps to add Puppeteer in the project of tests

#### 1) In `.\features\my_first.feature`, Include new step in the same scenario

````javascript
Given I write the url of portal 'https://www.google.com/'
````

#### 2) Create `.\features\steps\browser_open.step.js` file and include the next step definition

````javascript
const {Then} = require('cucumber');
const {expect} = require('chai');
Then('I run the first step using the text {string}', {timeout: 20 * 1000}, async (textValue) => {
    // console.log('textValue parameter', textValue);
    await expect(textValue).to.have.string('World');
});
````

#### 3) Now run the test and verify that chromium browser is opened

````bash
npm run test:e2e
````

#### 4) In `.\features\my_first.feature`, Include other step in the same scenario

````text
 Then I close the browser
````

#### 5) Create `.\features\steps\browser_close.step.js` file and include the step definition

````javascript
const {Then} = require('cucumber');
const {expect} = require('chai');

Then('I close the browser', {timeout: 20 * 1000}, async () => {
    await expect(await global.testContext.page.waitForSelector("body"));
    await global.testContext.browser.close();
});
````

#### 6) Run the test again

````bash
npm run test:e2e
````

----

## References

- https://github.com/sweekson/puppeteer-samples
- https://flaviocopes.com/puppeteer
- https://nitayneeman.com/posts/getting-to-know-puppeteer-using-practical-examples/
- http://thecodebarbarian.com/control-chrome-from-node-js-with-puppeteer.html
