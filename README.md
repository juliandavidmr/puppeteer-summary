# Puppeteer's summary

## Overview

- Documentation and useful resources
    - [Puppeteer API docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md)
    - [Puppeteer documentation](https://pptr.dev)
    - [Awesome puppeteer](https://github.com/transitive-bullshit/awesome-puppeteer)

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

| Function                               | Description                                  |
|----------------------------------------|----------------------------------------------|
| `puppeteer.launch({ headless:false })` |                                              |

#### `Browser`

> Base: `const browser = await puppeteer.launch()`

| Function                                     | Description                                    |
| -------------------------------------------- | ---------------------------------------------- |
| `browser.createIncognitoBrowserContext()`    | Creates a new browser context                  |
| `browser.close()`                            | Closes the browser with the default context    |

#### `Page`

> Base: `const page = await browser.newPage();`

| Function                                           | Description |
|----------------------------------------------------|-------------|
| `page.setViewport({ width: 600, height: 400 })`    | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagesetviewportviewport)            |
| `page.goto('https://website.com')`                 | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagegotourl-options)            |
| `page.hover('button#exampleId')`                   | [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagehoverselector) |
| `page.waitForSelector('body')`                     | Wait for the selector to appear in page. [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagewaitforselectorselector-options)    |
| `page.screenshot({path: 'screenshot.png'})`        | Promise which resolves to buffer or a base64 string (depending on the value of encoding) with captured screenshot. [Docs](https://github.com/puppeteer/puppeteer/blob/master/docs/api.md#pagescreenshotoptions) |


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

----

## References

- https://github.com/sweekson/puppeteer-samples
- https://flaviocopes.com/puppeteer
- https://nitayneeman.com/posts/getting-to-know-puppeteer-using-practical-examples/
