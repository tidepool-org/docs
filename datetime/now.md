## Handling browser-local datetimes in the present moment

The reason that we can't generally use JavaScript's built-in `Date` constructor at Tidepool—particularly when dealing with diabetes device data—is because JavaScript's `Date` only handles two types of datetime objects:

- browser-local datetimes (i.e., datetimes in the timezone that the browser detects the user is in)
- UTC datetimes

Since Tidepool generally deals with diabetes device datetimes in a user's configured, *arbitrary* timezone, JavaScript's `Date` does not provide the required functionality, and [Moment.js](https://momentjs.com/ 'Moment.js') must be used instead.

In the case of handling browser-local datetimes in the present moment, however, JavaScript's `Date` provides **exactly** what is needed[^a]: a browser-local "now."

For formatting a browser-local datetime, any tool may be used. @jebeck—and anyone else who has extensive experience with the `strftime` datetime-formatting API—may prefer [`d3-time-format`](https://github.com/d3/d3-time-format 'GitHub: d3-time-format`) over Moment[^b].

[^a]: Assuming, of course, that the user does not do something pathologically silly, like manually configure their computer to a different timezone that their geographically current timezone.

[^b]: @jebeck also think it is handy to use different libraries to help distinguish the very different requirements of handling diabetes device datetimes in an arbitrary timezone versus browser-local `now`s.
