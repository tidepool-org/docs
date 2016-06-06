## On "hypermodularization"

**April 2016**

This section of documentation is *exploratory*, intended for use as a catalyst for discussion about our front-end development strategy/strategies at Tidepool as we have recently concluded several projects (deprecating our [notes web app](https://github.com/tidepool-org/clamshell 'GitHub: clamshell') and migrating our remaining client applications, the [chrome-uploader](https://github.com/tidepool-org/chrome-uploader 'GitHub: chrome-uploader') and [blip](https://github.com/tidepool-org/blip 'GitHub: blip') to [redux](http://redux.js.org/ 'redux') for state management) and are preparing to embark on what's next in our development pipeline—potentially splitting out pieces of blip as appropriate for the work of becoming an OAuth provider and (most concretely at the time of this writing) incrementally rewriting our data visualization web components, currently residing entirely in the [tideline](https://github.com/tidepool-org/tideline 'GitHub: tideline') repository, which is published as a package to npm and included by blip as a dependency.

The recommended order for reading the various short documents in this section is:

- [What is hypermodularization?](what-is-hypermodular.md)
- [Recommended reading](recommended-reading.md)
- [Pros and cons of hypermodularization](pros-and-cons.md)
- [A proposal](proposal.md)
