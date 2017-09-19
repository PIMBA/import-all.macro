<div align="center">
<h1>import-all.macro</h1>

<p>A babel-macro that allows you to import all files that match a glob</p>
</div>

<hr />

[![Build Status][build-badge]][build]
[![version][version-badge]][package]
[![downloads][downloads-badge]][npmtrends]
[![MIT License][license-badge]][LICENSE]

[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors)
[![PRs Welcome][prs-badge]][prs]
[![Code of Conduct][coc-badge]][coc]
[![Babel Macro][macros-badge]][babel-macros]

[![Watch on GitHub][github-watch-badge]][github-watch]
[![Star on GitHub][github-star-badge]][github-star]
[![Tweet][twitter-badge]][twitter]

## The problem

You want to import all files that match a `glob` without having to import them
individually.

## This solution

This is a [babel-macro][babel-macros] which allows you to import files that
match a glob. It supports `import` statements for synchronous resolution as well
as dynamic `import()` for deferred resolution (for code splitting with react
router for example).

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and
should be installed as one of your project's `devDependencies`:

```
npm install --save-dev import-all.macro
```

## Usage

Once you've [configured `babel-macros`][babel-macros-user] you can
import/require `import-all.macro`.

The `importAll` functions accept a [`glob`][glob] and will transpile your code
to import statements/dynamic imports for each file that matches the given glob.

Let's imagine you have a directory called `my-files` with the files
`a.js`, `b.js`, `c.js`, and `d.js`.

Here are a few before/after examples:

<!-- SNAP_TO_README:START -->
<!-- This section is generated by the other/snap-to-readme.js script. -->
<!-- Do not edit directly. -->

**`importAll` uses dynamic import**

```javascript
import importAll from 'import-all.macro'

document.getElementById('load-stuff').addEventListener(() => {
  importAll('./my-files/*.js').then(all => {
    console.log(all)
  })
})

      ↓ ↓ ↓ ↓ ↓ ↓

document.getElementById('load-stuff').addEventListener(() => {
  Promise.all([
    import('./my-files/a.js'),
    import('./my-files/b.js'),
    import('./my-files/c.js'),
    import('./my-files/d.js'),
  ])
    .then(function importAllHandler(importVals) {
      return {
        a: importVals[0],
        b: importVals[1],
        c: importVals[2],
        d: importVals[3],
      }
    })
    .then(all => {
      console.log(all)
    })
})
```

**`importAll.sync` uses static imports**

```javascript
import importAll from 'import-all.macro'

const a = importAll.sync('./my-files/*.js')

      ↓ ↓ ↓ ↓ ↓ ↓

import * as _a from './my-files/a.js'
import * as _b from './my-files/b.js'
import * as _c from './my-files/c.js'
import * as _d from './my-files/d.js'

const a = {
  a: _a,
  b: _b,
  c: _c,
  d: _d,
}
```

**`importAll.deferred` gives an object with dynamic imports**

```javascript
import importAll from 'import-all.macro'

const routes = importAll.deferred('./my-files/*.js')

      ↓ ↓ ↓ ↓ ↓ ↓

const routes = {
  a: function a() {
    return import('./my-files/a.js')
  },
  b: function b() {
    return import('./my-files/b.js')
  },
  c: function c() {
    return import('./my-files/c.js')
  },
  d: function d() {
    return import('./my-files/d.js')
  },
}
```

<!-- SNAP_TO_README:END -->

## Caveats

Some static analysis tools (like ESLint, Flow, and Jest) wont like this very much
without a little additional work. So Jest's watch mode may not pick up all your
tests that are relevant based on changes and some ESLint plugins
(like `eslint-plugin-import`) will probably fail on this.

## Inspiration

[Sunil Pai's tweet][sunil-tweet]

## Other Solutions

I'm not aware of any, if you are please [make a pull request][prs] and add it
here!

## Contributors

Thanks goes to these people ([emoji key][emojis]):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
| [<img src="https://avatars.githubusercontent.com/u/1500684?v=3" width="100px;"/><br /><sub>Kent C. Dodds</sub>](https://kentcdodds.com)<br />[💻](https://github.com/kentcdodds/import-all.macro/commits?author=kentcdodds "Code") [📖](https://github.com/kentcdodds/import-all.macro/commits?author=kentcdodds "Documentation") [🚇](#infra-kentcdodds "Infrastructure (Hosting, Build-Tools, etc)") [⚠️](https://github.com/kentcdodds/import-all.macro/commits?author=kentcdodds "Tests") |
| :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors][all-contributors] specification.
Contributions of any kind welcome!

## LICENSE

MIT

[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[build-badge]: https://img.shields.io/travis/kentcdodds/import-all.macro.svg?style=flat-square
[build]: https://travis-ci.org/kentcdodds/import-all.macro
[coverage-badge]: https://img.shields.io/codecov/c/github/kentcdodds/import-all.macro.svg?style=flat-square
[coverage]: https://codecov.io/github/kentcdodds/import-all.macro
[version-badge]: https://img.shields.io/npm/v/import-all.macro.svg?style=flat-square
[package]: https://www.npmjs.com/package/import-all.macro
[downloads-badge]: https://img.shields.io/npm/dm/import-all.macro.svg?style=flat-square
[npmtrends]: http://www.npmtrends.com/import-all.macro
[license-badge]: https://img.shields.io/npm/l/import-all.macro.svg?style=flat-square
[license]: https://github.com/kentcdodds/import-all.macro/blob/master/LICENSE
[prs-badge]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[donate-badge]: https://img.shields.io/badge/$-support-green.svg?style=flat-square
[coc-badge]: https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]: https://github.com/kentcdodds/import-all.macro/blob/master/other/CODE_OF_CONDUCT.md
[macros-badge]: https://img.shields.io/badge/babel--macro-%F0%9F%8E%A3-f5da55.svg?style=flat-square
[babel-macros]: https://github.com/kentcdodds/babel-macros
[github-watch-badge]: https://img.shields.io/github/watchers/kentcdodds/import-all.macro.svg?style=social
[github-watch]: https://github.com/kentcdodds/import-all.macro/watchers
[github-star-badge]: https://img.shields.io/github/stars/kentcdodds/import-all.macro.svg?style=social
[github-star]: https://github.com/kentcdodds/import-all.macro/stargazers
[twitter]: https://twitter.com/intent/tweet?text=Check%20out%20import-all.macro%20by%20%40kentcdodds%20https%3A%2F%2Fgithub.com%2Fkentcdodds%2Fimport-all.macro%20%F0%9F%91%8D
[twitter-badge]: https://img.shields.io/twitter/url/https/github.com/kentcdodds/import-all.macro.svg?style=social
[emojis]: https://github.com/kentcdodds/all-contributors#emoji-key
[all-contributors]: https://github.com/kentcdodds/all-contributors
[babel-macros-user]: https://github.com/kentcdodds/babel-macros/blob/master/other/docs/user.md
[glob]: https://www.npmjs.com/package/glob
[sunil-tweet]: https://twitter.com/threepointone/status/908290510225330176
