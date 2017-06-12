### React and performance

A great deal of the motivation behind *how* React works under the hood **is** performance, and this makes the task of giving guidance on writing performant React applications both simpler *and* harder.

It's simpler because we can just point to all the work React is doing under the hood to make an application written using the framework fast. The biggest thing here is how React works around speed issues related to adding, removing, or updating elements in the browser's DOM[^a]. React tracks UI changes in an in-memory (i.e., JavaScript) virtual representation of the application's component tree, then uses a diffing algorithm to isolate just the smallest sets of updates that need to be made to reconcile the updated tree and the browser's DOM when the UI is updated.

It's harder to give guidance on writing performant React applications because the places where one can still run into performance issues with React are, at least some of them, somewhat subtle and require precise attention and thought. What follows here is a *non-exhaustive* list of React performance "gotchas" as well as strategies for mitigating each. This should be a living list that is updated by Tidepool developers as our usage of React (and React itself) continues to evolve.

1. While React helps you make changes to the DOM as efficiently as possible, other performance issues with the DOM are not improved, in particular limitations on **how many elements** can be rendered in the tree. In short: React doesn't change the fact that the browser can't smoothly render thousands of DOM elements. If you need to render a list with thousands of items to be scrolled through—or, in our case, many thousands of CGM sensor readings as SVG `<circle>`s—you'll probably want to look into a [virtual rendering](https://github.com/bvaughn/react-virtualized 'GitHub: react-virtualized') strategy of some sort to significantly reduce the number of elements being rendered in the DOM at any given time.
<!-- 1. TODO: add re: reducing wasted (i.e., no DOM changes) rendering cycles and discuss gotchas that lead to such waste (i.e., subtly changing props) -->

#### Recommended reading

- [A deep dive into React perf debugging](http://benchling.engineering/deep-dive-react-perf-debugging/ 'A deep dive into React perf debugging') from the Benchling engineering blog

[^a]: In our case, at least, since we're using React to write web applications that run in Google's Chrome browser. React can also be used to write native mobile applications, in which case the diffing algorithm determines the smallest set of updates needed to the native view(s).
