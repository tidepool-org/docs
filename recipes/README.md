## recipes & workflows for front-end engineers at Tidepool

This document collects some "recipes" and workflows for (mainly) front-end engineers at Tidepool.

#### Background

We're building a *platform* at Tidepool, and while in the past we had some solutions[^1] for isolating work on front-end code, we've found those solutions fragile and too difficult to maintain as a small team, and so in general (although it feels like "overkill") we front-end engineers at Tidepool often run the entire platform locally while we work. There are a few tips & tricks necessary to do this most efficiently and take full advantage of our front-end developer tools, like [webpack hot module replacement](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack 'GitHub: webpack wiki on HMR').

### A: Running the platform locally with runservers but blip with hot module replacement (HMR) via webpack

1. Start up the platform locally using runservers (cf. [starting services](http://developer.tidepool.io/starting-up-services/ 'Tidepool Developer Portal: Starting Services'))
1. Kill the blip process runservers that spun up with `kill $TP_blip`
1. In a fresh terminal tab or window, navigate to your local clone of blip and source the local config with `source config/local.sh`
1. Now start up blip with HMR using `npm start`
1. When you're done working, remember to `tp_kill -9` to properly dispose of all the processes started by runservers (a plain `tp_kill` will not get all of them!)

### B: Changing the `MINIMUM_UPLOADER_VERSION` in jellyfish locally

In the Chrome uploader, we check - both on launch of the app and at the start of any upload attempt - whether the user's uploader is at or above the `MINIMUM_UPLOADER_VERSION` configured in the [jellyfish](https://github.com/tidepool-org/jellyfish 'Tidepool on GitHub: jellyfish') data ingestion service. In order to work on the UI for the error displayed to a user when their uploader is out-of-date, you need to be able to change the `MINIMUM_UPLOADER_VERSION` jellyfish has configured, preferably without killing and restarting the entire runservers bundle of processes each time you make a change to the configuration! Here's how to do that:

1. Start up the platform locally using runservers
1. When you want to change the `MINIMUM_UPLOADER_VERSION`, open up the runservers file from your local copy of the [tools](https://github.com/tidepool-org/tools 'Tidepool on GitHub: tools') and comment out **all** the function *calls* starting with `tp_warmup` then `tp_mongo` etc. at the bottom of the file
1. Higher up in the file in the definition of the `tp_jellyfish` function, edit the `MINIMUM_UPLOADER_VERSION` as desired
1. Re-source the runservers file in your terminal with `source tools/runservers`
1. Restart the jellyfish service with `tp_restart jellyfish`[^2]

### C: Working on the UI for API error states in the uploader or blip

One of the nice things about running the whole platform locally is that it's fairly easy to work on the UI for the surfacing of API errors in our client apps. There are a couple strategies for this.

#### Modify tidepool-platform-client locally

Since API calls (currently!) go through the [tidepool-platform-client](https://github.com/tidepool-org/platform-client 'Tidepool on GitHub: platform-client'), you can just pull down a clone of platform-client and edit the appropriate API wrapping function(s) to always return an error. From within your client app directory, use the following commands to depend on your local platform-client instead of the version installed from npm:

```bash
$ rm -rf node_modules/tidepool-platform-client/
$ npm link ../platform-client
```

#### Modify a service locally

You can also modify one (or more) of the backend services locally to return an error response to whatever call(s) you're working on. @jebeck typically does this to work on surfacing an error from the check against jellyfish's /info endpoint (which returns jellyfish's configured `MINIMUM_UPLOADER_VERSION` among other things). The workflow for this is as follows:

1. Start up the platform locally using runservers
1. In your local copy of the service repo, make the change(s) to the endpoint(s) to return an error in response to all calls to that endpoint
1. Restart the service from your runservers session with `tp_restart $SERVICE`
1. When you're done, just clear your changes and `tp_restart $SERVICE` again to restore the normal behavior of the service

### D: Running against the `dev` environment

If you don't source a configuration file in blip, then when you use `npm start`, blip will configure itself by default for the `dev` environment. You still access blip (with HMR) at [http://localhost:3000](http://localhost:3000), but you can log in with any account you've set up on the `dev` environment. Depending on what you're working on, this is *almost* as good of a developer experience as running everything locally (just a bit slower, of course), and it's a good idea to set up a set of accounts for yourself on `dev` containing sample diabetes data from various devices and various configurations of account ownership and sharing relationships.

### E: Verifying an e-mail address without sending an e-mail

If you're running locally and don't have the ability to send an e-mail in order to
get the link to verify an e-mail address, you can still accomplish this by extracting the
relevant information out of your local MongoDB instance. Once you have gone through the
initial account sign-up process, we get into our DB to grab our `signupKey`.

```mongo
> use hydrophone
> db.confirmations.find({'email':<account e-mail address>}, {_id:1})
```
This should return you an object containing just the `_id` string you're interested in
(this is just an example):
```
{ "_id" : "uyo_2kWWRGLZ9i3xek2reAnBeTtWWvTZ" }
```
Copy that string and construct a URL as such: (again, just an example assuming you're
running Blip in a default configuration - use the id string you got from the database)

 ```
 http://localhost:3000/login?signupKey=uyo_2kWWRGLZ9i3xek2reAnBeTtWWvTZ
 ```
Open this URL in your browser and you can complete the signup for your new user with the
e-mail verified. If you wish to confirm a successful verification, you can check the
database to see that `status` on the confirmation is `completed`:
```mongo
> db.confirmations.find({'email':<account e-mail address>}, {status:1})
{ "_id" : "uyo_2kWWRGLZ9i3xek2reAnBeTtWWvTZ", "status" : "completed" }
```

Alternatively, very soon (i.e., once the custodial user account creation work is on master) you will also be able to extract the `signupKey` from your local server logs. The log message will look something like:

```
2016/04/20 10:00:00 Sending email confirmation to you@email.com with key DXGNhyfzbcLpxwJ7BvtrfeeTOcvgabcd
```

### Working with the Local Database

Working with your local mongo database is pretty easy via [the mongo Shell](https://docs.mongodb.org/manual/reference/mongo-shell/ 'mongo Shell Quick Reference'), but here are a couple of very common recipes to get started.

#### Deleting all local device data

The only tricky part here is the fact that the database containing diabetes device data is called `streams` on localhost. From the mongo shell (accessible with the `mongo` command when you have the whole platform running locally):

```mongo
> use streams
> db.deviceData.remove({});
```

To delete just a single user's data, also a common task, you can modify the object passed to `remove()` with the properties you want to match, such as `userid`.

#### Completely wiping your local mongo

If you ever want to start with a completely fresh mongo[^3], the following JavaScript, when saved as a local file, can be fed as the first argument to the `mongo` command while the whole platform is running in order to delete all Tidepool-generated databases.

```mongo
var mg = new Mongo();

var ourDbs = [
  'gate',
  'hydrophone',
  'local',
  'messages',
  'streams',
  'user',
  'oauth_test',
  'test_ingestion',
  'test_messages',
  'test_streams',
  'user_test'
];

for (var i = 0; i < ourDbs.length; ++i) {
  var thisDb = ourDbs[i];
  db = db.getSiblingDB(thisDb);
  db.dropDatabase();
}
```

[^1]: Blip and the Chrome uploader both had a "mock" mode using a mock version of the API (wrapper) that returned mock data via the [blip-mock-data](https://github.com/tidepool-org/blip-mock-data 'Tidepool on GitHub: blip-mock-data') repo and package published to npm. Due to recent and upcoming changes to the Tidepool platform API(s), we have deprecated this mock data repository and removed the mock mode option from both blip and the Chrome uploader.

[^2]: Obligatory "TP restart" GIF (kudos belong to @darinkrauss for bidirectionalizing the original): ![the `tp_restart` kitty](../images/tp_restart-kitty.gif)

[^3]: @jebeck's reason for doing this is typically because she's forgotten what uni- or bi-directional sharing relationships are set up among all her local accounts and would like to start fresh to test a specific configuration.
