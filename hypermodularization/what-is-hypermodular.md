## What is "hypermodularization"?

What I[^a] mean by this is a project (examples: [React](https://facebook.github.io/react/ 'Facebook Code: React'), [redux](http://redux.js.org/ 'redux')) that is explicitly designed with the expectation that a single dependency will **not** be self-contained. A developer-user (henceforth: user) will have to install several, perhaps even “many”[^b], dependencies in order to make full use of the project/framework or perhaps even to use it at all (as intended).

## Examples

### React

React has **not** been hypermodular from the beginning. It used to be a “monolith”[^c] with everything included in the `react` package, including utilities like [`classnames`](https://github.com/JedWatson/classnames 'GitHub: classnames') and [testing tools](https://www.npmjs.com/package/react-addons-test-utils 'npm: react-addons-test-utils'). But as the framework and ecosystem have developed, more and more has been broken out into separate libraries/dependencies, straight through to the division of the main React codebase into [`react`](https://www.npmjs.com/package/react 'npm: react') and [`react-DOM`](https://www.npmjs.com/package/react-dom 'npm: react-dom') (for DOM-specific rendering, as opposed to [`react-native`](https://www.npmjs.com/package/react-native 'npm: react-native'), for native mobile rendering). The React ecosystem makes updates in lockstep with every dependency updating to the same version number at the same time, so for example you will get warnings and/or errors on `npm install` if (the relevant bit of) your `package.json` looks like this:

```json
  {
    "react": "0.14.8",
    "react-addons-pure-render-mixin": "0.14.7",
    "react-addons-update": "0.14.6",
    "react-dom": "0.14.8"
  }
```

Rather than like this:

```json
  {
    "react": "0.14.8",
    "react-addons-pure-render-mixin": "0.14.8",
    "react-addons-update": "0.14.8",
    "react-dom": "0.14.8"
  }
```

### redux

In contrast to React, redux has been hypermodular from the very beginning (which wasn't that long ago with a [1.0.0](https://github.com/reactjs/redux/releases/tag/v1.0.0 'GitHub: redux v1.0.0') on August 14th, 2015). The main codebase is barely over 500 lines of code and hasn't changed all that much over time. In short: redux is a good example of a simple, elegant idea that provides just enough to *enable* an entire ecosystem of tools built on top of it/alongside it/[your preferred spacial metaphor here]. A typical redux implementation (in a React app) probably includes the following dependencies from the ecosystem:

- [`redux-thunk`](https://github.com/gaearon/redux-thunk 'GitHub: redux-thunk') or [`redux-saga`](https://github.com/yelouafi/redux-saga 'GitHub: redux-saga') for asynchronous and/or conditional action creators
- [`react-redux`](https://github.com/reactjs/react-redux 'GitHub: react-redux') for React bindings
- [`react-router-redux`](https://github.com/reactjs/react-router-redux 'GitHub: react-router-redux') for keeping React Router's state in sync with the redux store
- [`reselect`](https://github.com/reactjs/reselect 'GitHub: reselect') for memoization of derived state
- any number of developer tools (where the ecosystem *really* shines), including:
   + [`redux-logger`](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') for logging redux-related stuff for inspection in the console
   + the redux [DevTools](https://github.com/gaearon/redux-devtools 'GitHub: redux-devtools') in various possible configurations

Unlike the React ecosystem, the redux ecosystem does **not** publish versions of the component modules in lockstep, and in fact the ecosystem is much less controlled's than React's ecosystem, which is controlled exclusively by Facebook, though with a large and highly responsive open-source presence.

### lodash

The very popular JavaScript toolbelt library [lodash](https://lodash.com/ 'lodash') is an interesting example of hypermodularization because it may not be apparent that it *is* hypermodularized. Most users use lodash through the [`lodash`](https://www.npmjs.com/package/lodash 'npm: lodash') package, which includes all of the tiny modules that make up the toolbelt. However, all the public lodash methods are also available as [single-function dependencies](https://www.npmjs.com/browse/keyword/lodash-modularized 'npm: lodash-modularized'). Overall, lodash is a good example to consider here in order to push on the difference between "hypermodularized" and "spread out across many repositories." lodash is hypermodularized *without* being "fragmented" across many repositories. The hypermodularization is achieved within a single repository, with build and publish processes that allow single modules to be published independently of each other. In analogy more like redux than React, each small lodash module is versioned more or less independently of the others, although the major version number is kept consistent across all modules to match the "umbrella" module's major version.

[^a]: @jebeck authored this document.

[^b]: "Many," of course, being a highly subjective quantifier.

[^c]: Usually a term applied to describe a server architecture (as opposed to a microservices architecture). We are being metaphorical here. Be nice, don't be a pedant!
