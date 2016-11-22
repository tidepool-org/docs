### Dependencies

In addition to the [React](https://www.npmjs.com/package/react 'React on npm') and [React DOM](https://www.npmjs.com/package/react-dom 'ReactDOM on npm') core libraries, we have chosen the following dependencies from the React ecosystem for use across all Tidepool's React applications to handle various common use cases:

`*` = In development *only*.

#### Dev Tools

- `*` [react-hot-loader 3.x](https://github.com/gaearon/react-hot-loader/tree/next-docs 'react-hot-loader 3.x') for hot reloading during development (no manual refreshing in the browser needed after changing code)

#### Misc

- [react-redux](https://github.com/reactjs/react-redux 'GitHub: react-redux') for the official React bindings for Redux

#### Testing

- [babel-plugin-rewire](https://github.com/speedskater/babel-plugin-rewire 'GitHub: babel-plugin-rewire') for dependency injection into ES2015/ES6 modules (and, more specifically and most commonly in our Redux work, mocking API calls via "rewiring" the API wrapper that is passed through props (in blip) or as part of the initialization of the async action creators (in the uploader)) [Sidenote: `babel-plugin-rewire` essentially provides the same functionality, although via a different API, as [rewire](https://github.com/jhnns/rewire 'GitHub: rewire') does for ES5 JavaScript, which we also have as a dependency in our client web applications.]
- [enzyme](http://airbnb.io/enzyme/index.html 'Enzyme') in our newer React codebases for nice syntactic sugar making it easier to test React components
- [react-addons-test-utils](https://facebook.github.io/react/docs/test-utils.html 'React docs: Testing Utilities') as a dependency for Enzyme or on its own in our older React codebases for basic React testing utilities
