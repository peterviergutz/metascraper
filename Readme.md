
# metascraper

A library to easily scrape metadata from an article on the web using Open Graph metadata, regular HTML metadata, and series of fallbacks.

Works on the server and in the browser!


## Example

This metadata...

    {
      "author": "Ellen Huet",
      "date": "2016-05-24T18:00:03.894Z",
      "description": "The HR startups go to war.",
      "image": "https://assets.bwbx.io/images/users/iqjWHBFdfxIU/ioh_yWEn8gHo/v1/-1x-1.jpg",
      "publisher": "Bloomberg.com",
      "title": "As Zenefits Stumbles, Gusto Goes Head-On by Selling Insurance"
    }

...would be scraped from this article...

[![](/support/screenshot.png)](http://www.bloomberg.com/news/articles/2016-05-24/as-zenefits-stumbles-gusto-goes-head-on-by-selling-insurance)


## API

#### `scrapeUrl(url, [rules])`

```js
import { scrapeUrl } from 'metascraper/server'

const metadata = await scrapeUrl('http://www.bloomberg.com/news/articles/2016-05-24/as-zenefits-stumbles-gusto-goes-head-on-by-selling-insurance')
```

Scrapes the `url` with matching `rules`.

#### `scrapeHtml(html, [rules])`

```js
import { scrapeHtml } from 'metascraper/server'

const metadata = await scrapeHtml(html)
```

Scrapes the `html` string with matching `rules`.

#### `scrapeWindow(window, [rules])`

```js
import { scrapeWindow } from 'metascraper/browser'

const metadata = await scrapeWindow(window)
```

Scrapes the `window` object with matching `rules`.


## Rules

Scraping rules are just asynchonous functions that get passed the `window` object and return a promise that resolves with the value of the metadata. Like so:

```js
async function rule(window) {
  const el = window.document.title
  return el.textContent
}
```

_In the browser `window` will be the global you'd expect, and server-side it would be a JSDOM instance._

Passing `rules` is optional, and the defaults have been configured for the best results when scraping web articles. But if you want to tweak the results, or want to scrape for additional metadata, you can pass in additional rules, or completely overwrite them.

For an idea of how rules work, [check out the default rules](/lib/rules).
