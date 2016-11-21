## redux @ Tidepool

[redux](https://github.com/reactjs/redux 'GitHub: redux') is a lightweight, easily testable state container for JavaScript applications. It takes inspiration equally from (a) Facebook's [Flux](http://facebook.github.io/flux/ 'Facebook: Flux') application architecture (especially its emphasis on one-way data flow) and (b) functional programming, in particular [Elm](http://elm-lang.org/ 'Elm'), a functional programming language for building GUIs on the web. We have recently adopted it as our solution for state management in our client web applications at Tidepool - [blip](https://github.com/tidepool-org/blip 'GitHub: blip') and the Chrome [uploader](https://github.com/tidepool-org/chrome-uploader 'GitHub: chrome-uploader'). Our incremental rewrite of the Tidepool data visualization library, found in the new repository [viz](https://github.com/tidepool-org/viz 'GitHub: viz'), also includes among its exports redux action creators and reducers.

The redux documentation is very well-written. If you are unfamiliar with the library, we recommended starting with the redux [Intro](http://redux.js.org/docs/introduction/index.html 'redux docs: Intro') and [Basics](http://redux.js.org/docs/basics/index.html 'redux docs: Basics') docs to familiarize yourself with the standard redux vocabulary before reading more of the documentation here about our use of redux at Tidepool.

This documentation summarizes the design decisions[^a] that guide our usage of redux across all our client web applications at Tidepool. We expect all current and future developers at Tidepool to abide by these decisions and guidelines to the fullest extent possible in order to maintain a consistent developer experience across applications and foster a set of standards that will make maintenance, further development, and onboarding of additional developers and contributors as easy as possible.

### Table of Contents

We recommend reading the following documents in this order:

- [our redux-related dependencies](./dependencies.md)
- [custom middlewares](./custom-middlewares.md)
- [directory structure & naming](./directory-structure.md)
- [our standard for redux actions](./actions.md)
- [our strategies for redux testing](./testing.md)

[^a]: In case anyone wants to praise or assign blame, those responsible for these design decisions are largely @jebeck (Jana Beck) and @GordyD (Gordon Dent). @jebeck authored the original version of this documentation.
