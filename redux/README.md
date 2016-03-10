## redux @ Tidepool

[redux](https://github.com/reactjs/redux 'GitHub: redux') is a lightweight, easily testable state container for JavaScript applications. It takes inspiration equally from (a) Facebook's [Flux](http://facebook.github.io/flux/ 'Facebook: Flux') application architecture (especially its emphasis on one-way data flow) and (b) functional programming, in particular [Elm](http://elm-lang.org/ 'Elm'), a functional programming language for building GUIs on the web. We have recently adopted it as our solution for state management in our client web applications at Tidepool - [blip](https://github.com/tidepool-org/blip 'GitHub: blip') and the Chrome [uploader](https://github.com/tidepool-org/chrome-uploader 'GitHub: chrome-uploader'). It is also likely that the next version of our data visualization library [tideline](https://github.com/tidepool-org/tideline 'GitHub: tideline') will include redux as a dependency.

The redux documentation is very well-written. If you are unfamiliar with the library, we recommended starting with the redux [Intro](http://redux.js.org/docs/introduction/index.html 'redux docs: Intro') and [Basics](http://redux.js.org/docs/basics/index.html 'redux docs: Basics') docs to familiarize yourself with the standard redux vocabulary before reading more of the documentation here about our use of redux at Tidepool.

This documentation summarizes the design decisions[^a] that guide our usage of redux across all our client web applications at Tidepool. We expect all current and future developers at Tidepool to abide by these decisions and guidelines to the fullest extent possible in order to maintain a consistent developer experience across applications and foster a set of standards that will make maintenance, further development, and onboarding of additional developers and contributors as easy as possible.

### Basics

In addition to the redux core library, we have chosen the following dependencies from the redux ecosystem to handle various common use cases:

`*` = In development *only*.

#### Dev Tools

- `*` [redux-devtools](https://github.com/gaearon/redux-devtools 'GitHub: redux-devtools') to provide the framework for advanced redux action & state inspection and time travel
- `*` [redux-devtools-log-monitor](https://github.com/gaearon/redux-devtools-log-monitor 'GitHub: redux-devtools-log-monitor') nested in a [redux-devtools-dock-monitor](https://github.com/gaearon/redux-devtools-dock-monitor 'GitHub: redux-devtools-dock-monitor') for inspecting actions & state and for [time travel](https://youtu.be/xsSnOQynTHs 'YouTube: Dan Abramov's Hot Reloading with Time Travel talk from React Europe 2015')
- `*` [react-hot-loader](https://github.com/gaearon/react-hot-loader 'GitHub: react-hot-loader') for hot reloading in conjunction with the [HotModuleReplacementPlugin](https://github.com/webpack/docs/wiki/list-of-plugins#hotmodulereplacementplugin 'GitHub: webpack HotModuleReplacementPlugin') (in blip) and/or the [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html 'webpack dev server')
- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console

#### Middlewares

- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console
- [redux-thunk](https://github.com/gaearon/redux-thunk 'GitHub: redux-thunk') for asynchronous and/or conditional `dispatch`ing of actions
- client-side routing middleware via `syncHistory` from [react-router-redux 3.0.0](https://github.com/reactjs/react-router-redux/tree/3.0.0 'GitHub: react-router-redux 3.0.0')

**NB:** The integration between redux and [React Router](https://github.com/reactjs/react-router 'GitHub: React Router') has probably been the fastest moving component of the React + redux ecosystem (and probably the seat of the most controversy), and we are already(!) *out of date*. The [4.0.0 release of react-router-redux](https://github.com/reactjs/react-router-redux/releases/tag/v4.0.0 'GitHub: react-router-redux v4.0.0') has introduced radical breaking changes. We will likely update at some time in the near(ish) future.

#### Testing

- [babel-plugin-rewire](https://github.com/speedskater/babel-plugin-rewire 'GitHub: babel-plugin-rewire') for dependency injection into ES2015/ES6 modules (and, more specifically and most commonly in our redux work, mocking API calls via "rewiring" the API wrapper that is passed through props (in blip) or as part of the initialization of the async action creators (in the uploader)) [Sidenote: `babel-plugin-rewire` essentially provides the same functionality, although via a different API, as [rewire](https://github.com/jhnns/rewire 'GitHub: rewire') does for ES5 JavaScript, which we also have as a dependency in our client web applications.]
- [redux-mock-store](https://github.com/arnaudbenard/redux-mock-store 'GitHub: redux-mock-store') for testing asynchronous and conditional action creators (i.e., "thunks"; see [this recipe](http://redux.js.org/docs/recipes/WritingTests.html#async-action-creators 'redux.js.org: Writing Tests for async action creators') from the redux docs)

### Custom Middlewares

One of the great benefits of redux is the easy path it provides for writing middleware to perform various actions in response to some or all of the redux actions that are the source of all changes to the application's state tree. The open-source community provides some great middleware options like the [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') that we include behind an environment variable to assist in development.

If you're unfamiliar with the concept of middleware, the [section of the redux documentation dedicated to middleware](http://redux.js.org/docs/advanced/Middleware.html 'Redux ') is a well-written, easily-understood thing of beauty. Go read it.

For our use cases at Tidepool, we are currently employing two custom middlewares. Thus far, these are only in use in the Chrome uploader because a bit more refactoring will be necessary to use them in blip, but we hope to accomplish that as part of our gradual improvements to the blip codebase.

#### Metrics Middleware

The metrics middleware inspects each action that flows through it to see if the action has a `metric` object inside the `meta` property of the action. (See [Standardized Actions](#standardized-actions) below for an explanation of the `meta` property.) If the action does have an object at `meta.metric`, then the middleware calls the Tidepool metrics endpoint with data extracted from `metric.eventName` and `metric.properties`.

In the Chrome uploader, the intersection between "things we want metrics for" and "events that are already part of the redux implementation" is nearly perfect; the only exception is the need for an action creator connected to the link to launch blip in a browser window, which does not affect the uploader's state at all and so would not normally need an action creator for the reducers to respond to and effect change in the store.

#### Error Logging Middleware

The error logging middleware inspects each action that flows through it to see if the action has its `error` property set to `true`. If so, then the middleware calls the Tidepool metrics endpoint with data extracted from the action `payload` (which will be an `Error` object according to our [Standardized Actions](#standardized-actions)) as well as data extracted from `metric` inside the `meta` property as above.

### Directory Structure & Naming

We have made an effort to standardized our directory structure and naming of files across repositories where we have a redux implementation. This makes it far easier to switch between client apps and not get lost!

In blip, the path to all the redux-related code is `app/redux/`; in the Chrome uploader it is `lib/redux/`. Within `redux/` the directory structure is as follows:

- a folder `actions/` contains the files:
    + `async.js` for asynchronous and/or conditional action creators that depend on the `redux-thunk` middleware
    + `index.js` for exporting all the actions
    + `sync.js` for the (simple) synchronous action creators that do not require the `redux-thunk` middleware


 - a folder `constants/` contains the files:
    + `actionTypes.js` for, you guessed it, the action type constants
    + other files for constants as needed for errors, &c


 - a folder `containers/` contains the files:
    + for the "root" app component, in dev and prod versions (as the dev version includes code to render the redux devtools UI)
    + `DevTools.js` to render the redux devtools UI


 - a folder `reducers/` contains the files:
    + for the reducers (*quelle surprise!*) [Since we use the standard `combineReducers` included with the `redux` core library, our reducers are broken up into separate files representing branches of the state tree as appropriate to each application.]
    + `misc.js` is the name we use for the file containing a collection of very small reducers (that on their own don't merit a whole file)
    + `index.js` which contains the call to `combineReducers` and exports the combined set


 - a folder `store` contains the files:
    + for configuring dev and prod versions of the redux store
    + `index.js` for exporting the appropriate (dev or prod) store

Or in sum:

```
├── redux
│   ├── actions
│   │   ├── async.js
│   │   ├── index.js
│   │   └── sync.js
│   ├── constants
│   │   ├── actionTypes.js
│   │   ├── [&c]
│   ├── containers
│   │   ├── Root.dev.js
│   │   ├── Root.js
│   │   ├── Root.prod.js
│   │   └── DevTools.js
│   ├── reducers
│   │   ├── index.js
│   │   ├── misc.js
│   │   └── [&c]
│   └── store
│       ├── configureStore.dev.js
│       ├── configureStore.prod.js
│       └── index.js
```

### Standardized Actions

As a way of promoting and enforcing standards across our repositories/applications that use redux, we've decided to adopt the [flux standard action (FSA)](https://github.com/acdlite/flux-standard-action 'GitHub: Flux Standard Action') standard for our action objects. [The standard](https://github.com/acdlite/flux-standard-action#actions 'FSA: Actions') is very concise, logical, and easy-to-learn. In addition, there is a [`flux-standard-action`](https://www.npmjs.com/package/flux-standard-action 'npm: flux-standard-action') module published to npm that exposes an `isFSA` function for testing whether an object adheres to the standard. The existence of this utility was a big factor in our decision to adopt FSA since a standard that isn't (easily) enforceable isn't much good at all. Accordingly - and as noted in [Testing Strategies](testing.md 'Testing Strategies') - we include a test to prove that each action created by our action creators passes `isFSA` in our redux unit tests.

There has been [some controversy around the way that FSA handles errors](https://github.com/acdlite/flux-standard-action/issues/17 'GitHub: FSA issue #17'), and we at Tidepool agree with @gajus that it would be better to put the `Error` object in the `error` field of an action, leaving the `payload` free to contain other information that might be needed to effect a change in the app's store. (We note this here as a reminder to ourselves to check occasionally to see if the standard has changed...)

[^a]: In case anyone wants to praise or assign blame, those responsible for these design decisions are largely @jebeck (Jana Beck) and @GordyD (Gordon Dent). @jebeck authored the original version of this documentation.
