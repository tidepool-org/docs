### Custom Middlewares

One of the great benefits of redux is the easy path it provides for writing middleware to perform various actions in response to some or all of the redux actions that are the source of all changes to the application's state tree. The open-source community provides some great middleware options like the [redux-logger](https://github.com/fcomb/redux-logger 'GitHub: redux-logger') that we include behind an environment variable to assist in development.

If you're unfamiliar with the concept of middleware, the [section of the redux documentation dedicated to middleware](http://redux.js.org/docs/advanced/Middleware.html 'Redux ') is a well-written, easily-understood thing of beauty. Go read it.

For our use cases at Tidepool, we are currently employing two custom middlewares. Thus far, these are only fully in use in the Chrome uploader and used for just a few metrics and API errors in blip.

#### Metrics Middleware

The metrics middleware inspects each action that flows through it to see if the action has a `metric` object inside the `meta` property of the action. (See [Standardized Actions](#standardized-actions) below for an explanation of the `meta` property.) If the action does have an object at `meta.metric`, then the middleware calls the Tidepool metrics endpoint with data extracted from `metric.eventName` and `metric.properties`.

In the Chrome uploader, the intersection between "things we want metrics for" and "events that are already part of the redux implementation" is nearly perfect; the only exception is the need for an action creator connected to the link to launch blip in a browser window, which does not affect the uploader's state at all and so would not normally need an action creator for the reducers to respond to and effect change in the store.

In blip, the intersection between "things we want metrics for" and "events that are already part of the redux implementation" is much smaller, which is why we're only using a metrics middleware for [a few metrics](https://github.com/tidepool-org/blip/blob/master/app/redux/utils/trackingMiddleware.js 'GitHub: blip app/redux/utils/trackingMiddleware.js').

#### Error Logging Middleware

The error logging middleware inspects each action that flows through it to see if the action has its `error` property set to `true`. If so, then the middleware calls the Tidepool metrics endpoint with data extracted from the action `payload` (which will be an `Error` object according to our [Standardized Actions](#standardized-actions)) as well as data extracted from `metric` inside the `meta` property as above.
