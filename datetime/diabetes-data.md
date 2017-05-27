## Handling datetimes in diabetes data

#### Table of contents

- [Background](#background)
- [Handling diabetes data datetimes on ingestion](#handling-diabetes-data-datetimes-on-ingestion)
- [Handling diabetes data datetimes in the client(s)](#handling-diabetes-data-datetimes-in-the-clients)

### Background

The storage of datetimes on actual physical diabetes devices is, in short, 💩. If you can imagine a way to store datetimes badly[^a], we've probably seen it at Tidepool.

To start, no diabetes device that we know of stores the datetimes of events that occur on the device in UTC (the standard on 🌎) or in a fashion that is *anchored* to UTC, with the possible exception of an iPhone serving as a receiver for a Dexcom G5 CGM (depending on how hair-splitty you want to get about the term "diabetes device"). Since UTC is the 🌍 standard and the only standard that allows for *aligning* data from multiple devices on the same timeline *reliably*, this presents a problem.

At Tidepool, aligning data from multiple devices on the same timeline reliably is **core** to our product, so we have implemented an algorithm (known around these parts by the abbrevaition BtUTC) to infer the UTC datetime from the relative datetimes stored on diabetes devices. This algorithm adds several properties to each datum in the Tidepool data model, and for the purposes of *this* document, it's not the hows of the calculation of these properties that is important, but rather what a few of them represent and can be used for:

- `time` is the inferred, ISO 8601-formatted UTC datetime of the event
- `deviceTime` is an ISO 8601-formatted version of the original datetime stored on the diabetes device; that is, Tidepool only reformatted this value, but did not otherwise change it in any way
- `timezoneOffset` is the inferred timezone offset for the event

`deviceTime` is a relative value to the user's local timezone, but prior to running BtUTC, none of the timezone information needed to anchor the datetime on the absolute scale of UTC is available; rather, this information is hidden or latent. (@jebeck has a fondness for referring to such datetimes as "timezone-naïve," though this is a perhaps a too liberal stretch of the meaning of "naïve." 🤷‍♀️)

The latent timezone offset value accompanying a `deviceTime` is what is inferred by the BtUTC algorithm and then used to calculate the UTC `time` value. In other words, `time` is the result of taking a relative datetime in `deviceTime` and subtracting the inferred `timezoneOffset` in order to convert the datetime to the absolute UTC scale:

```
time = deviceTime - timezoneOffset
```

For example, a `deviceTime` of midnight on January 1st, 2017 for a user uploading then with their device aligning with the `US/Pacific` timezone would result in an inferred `timezoneOffset` of `-480`, and therefore:

```
2017-01-01T08:00:00.000Z = 2017-01-01T00:00:00 - -480
```

More precisely as a computer would perform this calculation, having parsed the first two String representations into integer hammertimes:

```
1483257600000 = 1483228800000 - -28800000
```

This equation is true in the vast majority of cases, but there are slightly more complex edge cases as well, but those are not relevant outside of the BtUTC algorithm itself, so the interested or needful reader should [refer to the BtUTC technical documentation](http://developer.tidepool.io/chrome-uploader/docs/BootstrappingToUTC.html 'Bootstrapping to UTC') for further details.

The *storage* protocols for datetimes on diabetes devices isn't even the only source of problems in handling them. The PwD's behavior in managing their devices' display time also causes complications. The user interfaces on many diabetes devices are not designed well, so aside from inattention, errors sometimes occur because they are *easier* to make in the interface than non-errors.

In general, it is good to approach diabetes device times with a very, very small set of assumptions. In other words, avoid all of [this list of assumptions](./falsehoods.md 'Falsehoods Tidepoolers (want to) believe about diabetes device times'), which have been proven false in one way or another (and often repeatedly) at Tidepool.

### Handling diabetes data datetimes on ingestion

If the only thing an engineer implementing the protocol to read data from a diabetes device needed to know about handling datetimes properly was "run the BtUTC algorithm," then there wouldn't be much of a need for this section. As the reader may have guessed already, BtUTC gets you most of the way there, but not all the way. There is at least one situation—plus BtUTC edge cases and bugs—that will still require the engineer working on data ingestion to manipulate diabetes device datetimes directly.

#### Schedule lookup

### Handling diabetes data datetimes in the client(s)


- - - - -

[^a]: Not pathologically or maliciously badly. Just, you know, *poorly*.
[^b]: Just kidding! We haven't actually seen this one yet at Tidepool. But it's the *only one*. Yes. *Really.*
[^c]: These are the two most common "default" times that we've seen on diabetes devices. These default times surface on events under various circumstances, the most common of which is a clock error due to extended lack of power to the device (dead battery or battery out too long).
