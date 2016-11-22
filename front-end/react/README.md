## React @ Tidepool

[React](https://facebook.github.io/react/ React) is a JavaScript library for building user interfaces developed by Facebook and supported as an open-source project. In contrast to other single-page app frameworks, React's model employs a *declarative* style of defining UI components and works best with a one-way data flow through the application (rather than two-way bindings). The [React docs](https://facebook.github.io/react/docs/installation.html 'https://facebook.github.io/react/docs/') and [official tutorial](https://facebook.github.io/react/tutorial/tutorial.html 'React tutorial') are good places to start if you're not familiar with React or React's concepts.

Because we've been using React at Tidepool since late 2013, which were its early days as a publicly released open-source library, our React codebase(s) are not the most consistent. In the intervening years, both technologies (e.g., ES2015/ES6) and React-specific best practices have evolved, but we haven't been able to evolve our entire codebase at the same time.

This documentation summarizes the design decisions that guide our usage of React across all our client web applications and internal dependencies at Tidepool. We expect all current and future developers at Tidepool to abide by these decisions and guidelines to the fullest extent possible **when writing new React code or making large changes to existing React code** in order to maintain a consistent developer experience across applications and foster a set of standards that will make maintenance, further development, and onboarding of additional developers and contributors as easy as possible.

#### Table of contents

We recommend reading the following documents in this order:

- [our recommended reading on React best practices](./recommended-reading.md)
- [our React-related dependencies](./dependencies.md)
- [our strategies for React testing](./testing.md)
