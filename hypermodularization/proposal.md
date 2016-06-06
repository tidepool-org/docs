## A proposal for hypermodularization at Tidepool

In this section, I'm[^a] going to lay out a proposal for a hypermodular implementation of the next version of our data visualization tooling for web applications at Tidepool (a.k.a. "tideline 2.0"). As part of this proposal, I hope to convince my readers that the cons listed in [the "Pros & Cons"](pros-and-cons.md) section either will not apply in this specific case (and why) or will apply only to a limited extent.

### What does "tideline 2.0" need?[^b]

(Numbers are for ease of reference, not an indicator of priority.)

1. screen size and media type responsiveness
1. deep linking to particular data views (view type, date range, options, &c)
1. new visualizations (CGM in trends view, daily view with two-finger scrolling)
1. (new) common logic and UI for navigating the time domain
1. data paging (but not urgently)
1. [maybe] data fetching and caching

### How would these needs be met through a "hypermodular" architecture?

I propose a *strictly* hierarchical project architecture with one-way data flow[^c], a clean separation of concerns by *kind* (data processing versus rendering, &c), and separation into separate repositories only where it makes sense.

If we call this new project[^d] "diaviz", the proposed pieces are:

- `diaviz` may or may not exist independently as a wrapper/intermediate library (see [recommended reading](recommended-reading.md)) that would be a dependency (of blip and/or for third parties), but we'd probably have the repo and use it as a home base for documentation, if nothing else

- `diaviz-scaffold` = developer tooling, a simple scaffold for rendering the project either in a fixed container with the size and dimensions of blip's current container for visualization or in a responsive container for working on the responsiveness of the new components
   + would be the **only** source of `react` and `react-*` dependencies
   + helps achieve (1) before blip's CSS is reworked for responsiveness


- `diaviz-data` = a set of utility modules for dealing with Tidepool's diabetes device data and encompassing a range of tasks/requirements including client-side pre-processing and computation of statistics

- `diaviz-state` = a state container employing [React Router](https://github.com/reactjs/react-router 'React Router'), [redux](http://redux.js.org/ 'redux'), and (in all likelihood) [crossfilter](http://square.github.io/crossfilter/ 'crossfilter') to store
   + would be the **only** source of `react-router`, `redux`, `redux-*`, and `crossfilter`, and `moment-timezone` dependencies
   + achieves (2) and would help with (5) and (6)


- `diaviz-nav` = a single React+redux "smart" component to render the (abstract) UI controlling navigation within the time domain (and potentially including visualization options), as well as a collection of "dumb" components for rendering the (non-abstract) UI
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere (e.g., `moment-timezone`)
   + achieves (4)


- `diaviz-daily` = a collection of "dumb" components and/or D3 charting modules for rendering the daily view
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere
   + helps to achieve (3)


- `diaviz-weekly` = a collection of "dumb" components and/or D3 charting modules for rendering the two-week view
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere
   + helps to achieve (3)


- `diaviz-trends` = a collection of "dumb" components and/or D3 charting modules for rendering the trends view
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere
   + helps to achieve (3)


- `diaviz-basics` = a collection of "dumb" components and/or D3 charting modules for rendering the basics view
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere
   + helps to achieve (3)


- `diaviz-settings` = a collection of "dumb" components for rendering the settings view(s)
   + would look "upwards" for its `react` and `react-*` dependencies and *not* include any other dependencies included elsewhere
   + helps to achieve (3)

### But what does it look like?



### Details on avoiding the dreaded Cons...

#### Boilerplate code (anti-DRYness)

#### Common dependencies

#### Complex deployment

#### Complex local(host) setup for developers

#### The Interface is a Nightmare :scream:

#### Many dependencies

[^a]: If you haven't figured out who by now...

[^b]: [Tidepool-internal GDocs reference](https://docs.google.com/document/d/1zACQThnrFmlcvxMIF2g_CnnkQ9A16ilw-JSI6oKgvoA/edit#heading=h.4t4q8lakp971)

[^c]: If you're thinking, "Where have I heard that before?" Probably [here](https://facebook.github.io/react/blog/2014/05/06/flux.html 'Facebook: Flux'). And [here](http://redux.js.org/ 'redux').

[^d]: In other words, *The Project Formerly Known As "tideline 2.0": Please Can Everyone Stop Calling It That?*
