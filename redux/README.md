## redux @ Tidepool

[redux](https://github.com/reactjs/redux 'GitHub: redux') is a lightweight, easily testable state container for JavaScript applications. We have recently adopted it as our solution for state management in our client web applications at Tidepool - [blip](https://github.com/tidepool-org/blip 'GitHub: blip') and the Chrome [uploader](https://github.com/tidepool-org/chrome-uploader 'GitHub: chrome-uploader'). It is also likely that the next version of our data visualization library [tideline](https://github.com/tidepool-org/tideline 'GitHub: tideline') will include redux as a dependency.

This documentation summarizes the design decisions that guide our usage of redux across all our client web applications at Tidepool. We expect all current and future developers at Tidepool to abide by these decisions and guidelines to the fullest extent possible in order to maintain a consistent developer experience across applications and foster a set of standards that will make maintenance, further development, and onboarding of additional contributors and developers as easy as possible.

### Basics

In addition to the redux core library, we have chosen the following dependencies from the redux ecosystem to handle various common use cases:

`*` = In development *only*.

#### Dev Tools

- `*` [redux-devtools](https://github.com/gaearon/redux-devtools 'GitHub: redux-devtools') to provide the framework for advanced redux inspection and time travel
- `*` [redux-devtools-log-monitor](https://github.com/gaearon/redux-devtools-log-monitor 'GitHub: redux-devtools-log-monitor') nested in a [redux-devtools-dock-monitor](https://github.com/gaearon/redux-devtools-dock-monitor 'GitHub: redux-devtools-dock-monitor') for inspecting application state and [time travel](https://youtu.be/xsSnOQynTHs 'YouTube: Dan Abramov's Hot Reloading with Time Travel talk from React Europe 2015')
- `*` [react-hot-loader](https://github.com/gaearon/react-hot-loader 'GitHub: react-hot-loader') for hot reloading in conjunction with the [HotModuleReplacementPlugin](https://github.com/webpack/docs/wiki/list-of-plugins#hotmodulereplacementplugin 'GitHub: webpack HotModuleReplacementPlugin') (in blip) and/or the [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html 'webpack dev server')
- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console

#### Middlewares

- `*` [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') to log actions as well as previous & resulting states in the JavaScript console
- [redux-thunk](https://github.com/gaearon/redux-thunk 'GitHub: redux-thunk') for asynchronous and conditional `dispatch`ing of actions
- client-side routing middleware via `syncHistory` from [react-router-redux 3.0.0](https://github.com/reactjs/react-router-redux/tree/3.0.0 'GitHub: react-router-redux 3.0.0')

**NB:** The integration between redux and [React Router](https://github.com/reactjs/react-router 'GitHub: React Router') has probably been the fastest moving component of the React + redux ecosystem, and we are already(!) *out of date*. The [4.0.0 release of react-router-redux](https://github.com/reactjs/react-router-redux/releases/tag/v4.0.0 'GitHub: react-router-redux v4.0.0') has introduced radical breaking changes. We will likely update at some time in the near(ish) future.

#### Testing

- [babel-plugin-rewire](https://github.com/speedskater/babel-plugin-rewire 'GitHub: babel-plugin-rewire') for dependency injection into ES2015/ES6 modules (and, more specifically and most commonly in our redux work, mocking API calls) [`babel-plugin-rewire` essentially provides the same functionality, although via a different API, as [rewire](https://github.com/jhnns/rewire 'GitHub: rewire') does for ES5 JavaScript, which we also have as a dependency in our client web applications.]
- [redux-mock-store](https://github.com/arnaudbenard/redux-mock-store 'GitHub: redux-mock-store') for testing asynchronous and conditional action creators (i.e., "thunks"; see [this recipe](http://redux.js.org/docs/recipes/WritingTests.html#async-action-creators 'redux.js.org: Writing Tests for async action creators') from the redux docs)
