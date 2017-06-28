### Falsehoods Tidepoolers (want to) believe about diabetes device times

- the display time on a PwD's device will be accurate to their local time
- OK, maybe not, but it will be pretty close (within minutes)
- OK, maybe sometimes it will be an hour off because of a DST changeover
- OK, maybe not, but the PwD will definitely remember to change the display time before the *next* DST changeover
- OK, maybe not, but the display time on a PwD's device won't be a month off
- OK, maybe not, but it won't be a year off
- OK, maybe not, but it won't be a decade off[^a]
- OK, maybe not, but it won't be in the future
- OK, maybe not, but it won't possibly ever be January 1st, 2007 or January 1st, 2008 if the PwD didn't get the device until 2015, *surely*[^b]
- no two (or more) events will share the *exact* same datetime
- if a stored time field on the events from a diabetes device *looks like* a Unix time, it is a Unix time


- - - - -

[^a]: Just kidding! We haven't actually seen this one yet at Tidepool. But it's the *only one*. Yes. *Really.*
[^b]: These are the two most common "default" times that we've seen on diabetes devices. These default times surface on events under various circumstances, the most common of which is a clock error due to extended lack of power to the device (dead battery or battery out too long).
