### Dependencies

In addition to the Redux core library, we have chosen the following dependencies from the Redux ecosystem for use across all Tidepool's Redux applications to handle various common use cases:

`*` = In development *only*.

#### Dev Tools

- `*` [redux-devtools](https://github.com/gaearon/redux-devtools 'GitHub: redux-devtools') to provide the framework for advanced Redux action & state inspection and time travel
- `*` [redux-devtools-log-monitor](https://github.com/gaearon/redux-devtools-log-monitor 'GitHub: redux-devtools-log-monitor') nested in a [redux-devtools-dock-monitor](https://github.com/gaearon/redux-devtools-dock-monitor 'GitHub: redux-devtools-dock-monitor') for inspecting actions & state and for [time travel](https://youtu.be/xsSnOQynTHs 'YouTube: Dan Abramov's Hot Reloading with Time Travel talk from React Europe 2015')
- `*` [react-hot-loader](https://github.com/gaearon/react-hot-loader 'GitHub: react-hot-loader') for hot reloading in conjunction with the [HotModuleReplacementPlugin](https://github.com/webpack/docs/wiki/list-of-plugins#hotmodulereplacementplugin 'GitHub: webpack HotModuleReplacementPlugin') (in blip) and/or the [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html 'webpack dev server')
- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console

#### Middlewares

- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console
- [redux-thunk](https://github.com/gaearon/redux-thunk 'GitHub: redux-thunk') for asynchronous and/or conditional `dispatch`ing of actions
- client-side routing middleware via `syncHistory` from [react-router-redux 3.x](https://github.com/reactjs/react-router-redux/tree/3.0.0 'GitHub: react-router-redux 3.x')
- [planned] [redux-worker-middleware](https://github.com/keyanzhang/redux-worker-middleware 'GitHub: redux-worker-middleware') for management of communication between the main app and one or more Web Workers through redux actions

#### Misc

<!-- TODO: add the right link for "Blip's usage of React Router" -->

- [react-redux](https://github.com/reactjs/react-redux 'GitHub: react-redux') for the official React bindings for Redux
- [react-router-redux 3.x](https://github.com/reactjs/react-router-redux/tree/3.0.0 'GitHub: react-router-redux 3.x') for integration between the Redux store and client-side router state (see [Blip's usage of React Router]() for details)
- [react-addons-update](https://facebook.github.io/react/docs/update.html 'React docs: Immutability Helpers') for ensuring that state transformations performed in our reducers don't *mutate* existing state but rather create new objects, etc. as necessary, essentially mimicking immutable data structures

**NB:** The integration between Redux and [React Router](https://github.com/reactjs/react-router 'GitHub: React Router') has probably been the fastest moving component of the React + Redux ecosystem (and probably the seat of the most controversy), and we are already(!) *out of date*. The [4.0.0 release of react-router-redux](https://github.com/reactjs/react-router-redux/releases/tag/v4.0.0 'GitHub: react-router-redux v4.0.0') has introduced radical breaking changes. We will likely update at some time in the near(ish) future.

#### Testing

- [babel-plugin-rewire](https://github.com/speedskater/babel-plugin-rewire 'GitHub: babel-plugin-rewire') for dependency injection into ES2015/ES6 modules (and, more specifically and most commonly in our Redux work, mocking API calls via "rewiring" the API wrapper that is passed through props (in blip) or as part of the initialization of the async action creators (in the uploader)) [Sidenote: `babel-plugin-rewire` essentially provides the same functionality, although via a different API, as [rewire](https://github.com/jhnns/rewire 'GitHub: rewire') does for ES5 JavaScript, which we also have as a dependency in our client web applications.]
- [redux-mock-store](https://github.com/arnaudbenard/redux-mock-store 'GitHub: redux-mock-store') for testing asynchronous and conditional action creators (i.e., "thunks"; see [this recipe](http://redux.js.org/docs/recipes/WritingTests.html#async-action-creators 'redux.js.org: Writing Tests for async action creators') from the Redux docs)
- our own [tidepool-standard-action](https://github.com/tidepool-org/tidepool-standard-action 'GitHub: tidepool-standard-action'), essentially our own maintained fork of [flux-standard-action](https://github.com/acdlite/flux-standard-action 'GitHub: flux-standard-action') with the major (and for us, *crucial*) difference that [a "Tidepool standard action" (TSA) includes a payload in an error action](https://github.com/tidepool-org/tidepool-standard-action#error-example 'TSA README: error example')
- our own [object-invariant-test-helper](https://github.com/tidepool-org/object-invariant-test-helper 'GitHub: object-invariant-test-helper'), a fork of the [redux-immutable-state-invariant](https://github.com/leoasis/redux-immutable-state-invariant 'GitHub: redux-immutable-state-invariant') development middleware, with API changes to focus on our use case of testing that none of our array or object reducers *mutate* the array or object but rather return a new array or object in every instance
