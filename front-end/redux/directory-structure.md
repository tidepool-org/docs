### Directory Structure & Naming

We have made an effort to standardized our directory structure and naming of files across repositories where we have a redux implementation. This makes it far easier to switch between client apps and not get lost!

In blip, the path to all the redux-related code is `app/redux/`; in the uploader it is `lib/redux/`. Within `redux/` the directory structure is as follows:

- a folder `actions/` contains the files:
    + `async.js` for asynchronous and/or conditional action creators that depend on the `redux-thunk` middleware
    + `index.js` for exporting all the actions
    + `sync.js` for the (simple) synchronous action creators that do not require the `redux-thunk` middleware


 - a folder `constants/` contains the files:
    + `actionTypes.js` for, you guessed it, the action type constants
    + other files for constants as needed for errors, &c


 - a folder `containers/` contains the files:
    + for the "root" app component, in dev and prod versions (as the dev version includes code to render the redux devtools UI)
    + `DevTools.js` to render the redux devtools UI


 - a folder `reducers/` contains the files:
    + for the reducers (*quelle surprise!*) [Since we use the standard `combineReducers` included with the `redux` core library, our reducers are broken up into separate files representing branches of the state tree as appropriate to each application.]
    + `misc.js` is the name we use for the file containing a collection of very small reducers (that on their own don't merit a whole file)
    + `index.js` which contains the call to `combineReducers` and exports the combined set


 - a folder `store` contains the files:
    + for configuring dev and prod versions of the redux store
    + `index.js` for exporting the appropriate (dev or prod) store

Or in sum:

```
├── redux
│   ├── actions
│   │   ├── async.js
│   │   ├── index.js
│   │   └── sync.js
│   ├── constants
│   │   ├── actionTypes.js
│   │   ├── [&c]
│   ├── containers
│   │   ├── Root.dev.js
│   │   ├── Root.js
│   │   ├── Root.prod.js
│   │   └── DevTools.js
│   ├── reducers
│   │   ├── index.js
│   │   ├── misc.js
│   │   └── [&c]
│   └── store
│       ├── configureStore.dev.js
│       ├── configureStore.prod.js
│       └── index.js
```
