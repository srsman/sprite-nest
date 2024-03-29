#  Sprite Nest

Crawl contents of specific types from web easily and flexibly.

## Feature

* Built-in specified type content Crawler
* Cluster execution(powered by [pupputeer-cluster](https://github.com/thomasdondorf/puppeteer-cluster))
* Logger information (powered by [winstom](https://github.com/winstonjs/winston))

## Install

```bash
yarn add sprite-nest
# or
npm install sprite-nest --save
```

## Usage

### Basic Usage

For example I would like to get the searching results of a keyword in multiple search engines.

```js
import SpriteNest from 'sprite-nest'
// or
// const SpriteNest = require('sprite-nest').default

const { Crawler } = SpriteNest

const keyword = 'ishihara satomi'

Crawler.search(keyword, ['google', 'baidu', 'duckduckgo']).then(res => {
  console.log(`result of searching ${keyword}: `, res)
});
```

### Use with Config

Later I hope to get translation results of text content in multiple translators and compare them. Furthermore set a **maximum concurrent crawler number** and enable **monitor** of cluster status, with a **visible browser window**.

```js
import SpriteNest from 'sprite-nest'
const { Crawler } = SpriteNest

const sentence = 'good morning'

Crawler.translator(sentence, ['google', 'baidu', 'bing'], {
  // Set config of puppeteer-cluster
  cluster: { 
    maxConcurrency: 3,
    monitor: true,
  },
  // Set config of puppeteer
  pptr: { 
    headless: false
  }
}).then(res => {
  console.log(`result of translating ${keyword}: `, res)
})
```

> SpriteNest uses **puppeteer-core** in default.

### Use Sprite and Puppeteer Page

`Crawler` contains wrapped task and `Sprite` is the class which has inner functions with some puppeteer operations, it means you can input a custom Page instance and then will get crawled data.

And now I get information of **Armani lipsticks** to pick one!

```js
import SpriteNest from 'sprite-nest'
import puppeteer from 'puppeteer'

const { Sprite } = SpriteNest;

const UseInnerSprite = async () => {
    const armaniSprite = new Sprite.LipStick.armani()
    const browser = await puppeteer.launch()
    const page = await browser.newPage()
    return await armaniSprite.getList({ page })
}

UseInnerSprite().then(lipsticks => console.log('armani lipsticks: ', lipsticks));
```

The exmaple above only get the basic information of lipsticks. In built-in lipstick task, it also get detail information of lipstick, like palette colors and names.

## API

### Crawler

#### `Crawler.lipstick(brands, config)`

* `brands` <string[]>
* `config` <?CustomConfig>

#### `Crawler.search(keyword, platforms, config)`

* `keyword` \<string\>
* `platforms` <string[]>
* `config` <?CustomConfig>

#### `Crawler.sns(target, account, platform, config)`

* `target` \<string\>
* `account` \<Object\>
  * `username` \<string\>
  * `password` \<string\>
* `brands` <string[]>
* `config` <?CustomConfig>

#### `Crawler.translator(content, platforms, config)`

* `content` \<string\>
* `platforms` <string[]>
* `config` <?CustomConfig>

### Sprite

Every sprite may has `getList()` or `getDetail()` method, depends on its type.

#### Lipstick

- [x] armani
- [x] covergirl
- [ ] cpb
- [x] dior
- [x] esteelauder
- [x] givenchy
- [ ] guerlian
- [x] lancome
- [ ] louboutin
- [x] mac
- [x] maquillage
- [x] maybelline
- [ ] nars
- [ ] sephora
- [x] tomford
- [x] urbandecay
- [x] ysl

#### Search

- [x] baidu
- [x] bing
- [x] duckduckgo
- [x] google

#### Translator

- [x] baidu
- [x] bing
- [x] google

#### SNS

- [x] weibo 

## Todo

- [ ] testing
- [ ] custom sprite constructor

## License

MIT
