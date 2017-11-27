## Redux @ Tidepool

[Redux](https://github.com/reactjs/redux 'GitHub: Redux') is a lightweight, easily testable state container for JavaScript applications. It takes inspiration equally from (a) Facebook's [Flux](http://facebook.github.io/flux/ 'Facebook: Flux') application architecture (especially its emphasis on one-way data flow) and (b) functional programming, in particular [Elm](http://elm-lang.org/ 'Elm'), a functional programming language for building GUIs on the web. We have recently adopted it as our solution for state management in our client web applications at Tidepool - [blip](https://github.com/tidepool-org/blip 'GitHub: blip') and the [uploader](https://github.com/tidepool-org/uploader 'GitHub: uploader'). Our incremental rewrite of the Tidepool data visualization library, found in the new repository [viz](https://github.com/tidepool-org/viz 'GitHub: viz'), also includes among its exports Redux action creators and reducers.

The Redux documentation is very well-written. If you are unfamiliar with the library, we recommended starting with the Redux [Intro](http://redux.js.org/docs/introduction/index.html 'Redux docs: Intro') and [Basics](http://redux.js.org/docs/basics/index.html 'Redux docs: Basics') docs to familiarize yourself with the standard Redux vocabulary before reading more of the documentation here about our use of Redux at Tidepool.

It is also very important to read and understand the section of the Redux documentation dedicated to [Normalizing State Shape](http://redux.js.org/docs/recipes/reducers/NormalizingStateShape.html 'Redux docs: Normalizing State Shape'). A properly "normalized" state tree—in brief: storing detail information in only *one* place in state for each entity that can be referenced by an ID—is very important to reap all the benefits of Redux, in particular for having confidence that for any particular piece of state, there is a *single* source of truth for that state in the application.

This documentation summarizes the design decisions[^a] that guide our usage of Redux across all our client web applications and internal dependencies at Tidepool. We expect all current and future developers at Tidepool to abide by these decisions and guidelines to the fullest extent possible in order to maintain a consistent developer experience across applications and foster a set of standards that will make maintenance, further development, and onboarding of additional developers and contributors as easy as possible.

#### Table of contents

We recommend reading the following documents in this order:

- [our Redux-related dependencies](./dependencies.md)
- [custom middlewares](./custom-middlewares.md)
- [directory structure & naming](./directory-structure.md)
- [our standard for Redux actions](./actions.md)
- [our strategies for Redux testing](./testing.md)

* * * * *

[^a]: In case anyone wants to praise or assign blame, those responsible for these design decisions are largely @jebeck (Jana Beck) and @GordyD (Gordon Dent). @jebeck authored the original version of this documentation.
