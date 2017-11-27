## front-end @ Tidepool

#### Table of contents

- [Repositories and published packages](#repositories-and-published-packages)
- [Common tech stack and usage](#common-tech-stack-and-usage)
    - [React @ Tidepool](./react/README.md)
    - [Redux @ Tidepool](./redux/README.md)
- [Developer guides](#developer-guides)
    - [blip](http://developer.tidepool.org/blip/docs/StartHere.html 'Tidepool developer portal: Blip developer guide')
    - [uploader](http://developer.tidepool.org/uploader/docs/StartHere.html 'Tidepool developer portal: uploader developer guide')
    - [viz](http://developer.tidepool.org/viz/docs/StartHere.html 'Tidepool developer portal: @tidepool/viz developer guide')
- [Miscellaneous recipes](./recipes.md)

<!-- TODO: add further links to individual developer guides -->

* * * * *

### Repositories and published packages

Tidepool has several repositories containing client-side JavaScript code in support of two client applications on the Tidepool platform, [blip](https://blip.tidepool.org/ 'Blip') and the [Tidepool uploader](https://tidepool.org/products/tidepool-uploader/). The GitHub repositories containing the code for these two applications are:

- [blip](https://github.com/tidepool-org/blip 'GitHub: blip')
- [uploader](https://github.com/tidepool-org/uploader 'GitHub: uploader')
- [tideline](https://github.com/tidepool-org/tideline 'GitHub: tideline'): data visualization code (in the process of being deprecated)
- [viz](https://github.com/tidepool-org/viz 'GitHub: viz'): data visualization code (an incremental rewrite of tideline that is currently in progress)
- [platform-client](https://github.com/tidepool-org/platform-client 'GitHub: platform-client'): Tidepool platform API utilities (to be deprecated eventually, but replacement efforts not yet started)
- [sundial](https://github.com/tidepool-org/sundial 'GitHub: sundial'): a thin wrapper around the [Moment.js](http://momentjs.com/ 'Moment.js') JavaScript datetime library (to be deprecated eventually and not used as an dependency in viz, but other replacement efforts have not yet been started)

For the four of these repositories that are internal dependencies rather than application code, we publish the repository code to the [node package manager (npm)](https://www.npmjs.com/ 'node package manager'), which we also use to manage our external JavaScript dependencies:

- [tideline on npm](https://www.npmjs.com/package/tideline 'tideline on npm')
- [sundial on npm](https://www.npmjs.com/package/sundial 'sundial on npm')
- [tidepool-platform-client on npm](https://www.npmjs.com/package/tidepool-platform-client 'platform-client on npm')
- [@tidepool/viz on npm](https://www.npmjs.com/package/@tidepool/viz '@tidepool/viz on npm')

The newest of these—@tidepool/viz—uses the `@tidepool` [npm "scope", a new(ish) feature](https://docs.npmjs.com/getting-started/scoped-packages 'npm: scoped packages'). Any future internal dependencies also published to npm should also use the @tidepool scope.

### Common tech stack and usage

Our client applications at Tidepool are single-page applications using [React](https://facebook.github.io/react/ 'React') with [Redux](http://redux.js.org/ 'Redux') for state management. For information on our general approach to React and Redux development at Tidepool, please read the following:

- [React @ Tidepool](./react/)
- [Redux @ Tidepool](./redux/)

### Developer guides

For each Tidepool client application and currently active dependent repository, please see the appropriate developer guide for more information about each project's architecture, usage of dependencies, etc.:

- [blip developer guide](http://developer.tidepool.org/blip/docs/StartHere.html 'Tidepool developer portal: Blip developer guide')
- [Tidepool uploader developer guide](http://developer.tidepool.org/uploader/docs/StartHere.html 'Tidepool developer portal: uploader developer guide')
- [@tidepool/viz developer guide](http://developer.tidepool.org/viz/docs/StartHere.html 'Tidepool developer portal: @tidepool/viz developer guide')
