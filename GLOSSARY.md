<!-- NB: the first paragraph below each ## glossary heading will be provided in hover text wherever the glossary term appears in our documentation, so keep the first paragraph as concise as possible and limited to the core essential info -->

## BtUTC

Bootstrapping to UTC (BtUTC) is the algorithm Tidepool uses to infer UTC datetimes from the relative datetimes stored on diabetes devices in combination with the timezone of the most recent data on the device and the set of display time settings changes on the device.

The [technical documentation for BtUTC](http://developer.tidepool.io/chrome-uploader/docs/BootstrappingToUTC.html 'Bootstrapping to UTC') resides in the uploader's docs, and there is also [a less technical blog post](http://tidepool.org/all/its-a-matter-of-time/ 'It's a Matter of Time').

- - - - -

## calendar date

An object involving only date information—year, month, and day—and referring to a date in the 365-day Gregorian calendar; represented best in ISO 8601 `YYYY-MM-DD` format.

The 365 day calendar is an abstraction that does not _quite_ line up to universal time (i.e., UTC) because it guarantees exactly 365 days in a typical year and 366 in a leap year, while universal time does not (there are also leap seconds, etc. which add up to slightly more than 365 or 366 days). As a concept outside of universal time, a calendar date does not need or include timezone information.

#### examples

`2016-01-10` is a calendar date. (In particular, it is the date David Bowie died... 😢)

Examples of calendar dates in Tidepool's code are the two dates stored in a PwD's profile: `birthday` and `diagnosisDate`.

- - - - -

## clock time

An object involving only time information—hours, minutes, seconds, and possibly milliseconds—and referring to a point within a standard twenty-four hour day.

#### examples

`6:05 p.m.` is a clock time in non-ISO 8601 format. (`18:05:00` would be the ISO 8601-formatted equivalent.) `64805000` is the same clock time in milliseconds.

In Tidepool front-end code, clock time is often computed from a datetime and then stored in milliseconds as `msPer24`.

- - - - -

## datetime

A "datetime" is our catchall term for an object involving *both* clock time information—hours, minutes, seconds, and possibly milliseconds—*and* date information—year, month, and day.

The terms "time" and "timestamp" can be problematic and potentially confusing because they don't transparently represent the inclusion of date information. Thus, for maximum precision and clarity, we recommend using the term "datetime" when both clock time and date information are part of the object in question, both in documentation and in code.

We have borrowed this terminology with love from [the `datetime` package in Python's standard library](https://docs.python.org/2/library/datetime.html 'Python: datetime').

#### examples

The concept of *right now, today, as you are reading this* is a datetime. 😉

`deviceTime` and `time` are both properties in the Tidepool data model that encode datetimes.

- - - - -

## display time

A display time is a type of *relative* datetime---that is, a datetime without timezone information and therefore **not** anchored to UTC.

#### examples

If you are a PwD, look at one of your diabetes devices right now; find the datetime display. Congratulations, unless you are looking at the time display on your iPhone for a Dexcom G5 system, you are looking at a display time.

- - - - -

## DST

Daylight saving time (abbreviated DST) is the practice of advancing clock time by an hour during the summer months so that evening daylight lasts longer.

Many timezones throughout the world observe DST, but some do not. Local governments decide exactly when the shift to and away from DST occurs.

#### examples

The major timezones—Pacific, Mountain, Central, and Eastern—in the continental United States all observe DST according to a federally-legislated schedule. However, most of Arizona and Hawaii do not observe DST.

- - - - -

## hammertime

A Unix time in milliseconds instead of seconds.

#### examples

`0` is the hammertime representing midnight on January 1st, 1970. `1495759428000` is the hammertime representing the time just before I typed *this*.

Presently there are no examples of hammertimes in the data Tidepool stores, but new front-end data visualization code parses each ISO-formatted timestamp (i.e., `time`) into a hammertime during the data preprocessing because repeatedly parsing String timestamps is very expensive in terms of performance.

- - - - -

## ISO 8601

ISO 8601 is the International Organization for Standardization's (ISO) standard covering the exchange of date- and time-related data.

You can read about it [on Wikipedia](https://en.wikipedia.org/wiki/ISO_8601 'Wikipedia: ISO 8601')[^a].

Tidepool uses ISO 8601 datetime formatting for calendar dates and for both relative ("timezone-naïve") and timezone-aware or UTC datetimes.

#### examples

`2017-05-25T23:48:52+00:00` is an ISO 8601-formatted datetime with the timezone offset of 0 (UTC) provided in the optional +/- offset notation.

`2017-W21` is an ISO 8601-formatted week, an example of an ISO 8601 specification that is *not* currently relevant or used by Tidepool.

At Tidepool, we use the Zulu format with milliseconds—i.e., `2017-05-25T23:48:52.000Z`—for the `time` field. For the relative datetime stored in the `deviceTime` field, we use the date and time formatting specifications to second precision——that is to say, `2017-05-25T23:48:52`.

- - - - -

## PwD

"Person with diabetes" or at Tidepool, equivalently, "Person with data."

This phrase (and its abbreviation PwD) is used in order to avoid the disease-centric term "diabetic." It is very commonly, though not universally, used throughout the diabetes community.

- - - - -

## timezone

A timezone—sometimes referred to as a named timezone or `timezoneName` at Tidepool for clarity—is a String value referring to a timezone from the [IANA Time Zone Database](https://www.iana.org/time-zones 'IANA Time Zone Database').

**NB**: An abbreviation such as `PDT` for "Pacific Daylight Time" is **not** a timezone since it contains both timezone *and* timezone offset information!

#### examples

`US/Pacific` is the timezone where Tidepool's headquarters are located. `Pacific/Easter` is the timezone for the Easter Islands. More timezone examples can be seen by hovering on the map below the fold on [Moment Timezone's landing page](https://momentjs.com/timezone/ 'Moment Timezone').

The `upload` type in Tidepool's data model stores a timezone in—wait for it...—the `timezone` property.

- - - - -

<!-- sadly, GitBook's glossary function does not currently recognize glossary terms that include other terms, so "timezone offset" will not yet be highlighted with its definition. there is an issue filed for it on GitHub, so maybe this will change someday... -->
## timezone offset

A timezone offset is a positive or negative integer representing an offset from UTC in minutes.

Timezones and timezone offsets do **not** map to each other one-to-one because of DST. For example, the `US/Pacific` timezone has a timezone offset of -420 during DST and a timezone offset of -480 when standard time is in effect. Be very careful, both in thought and in code, to keep these concepts of timezone and timezone offset distinct!

#### examples

The timezone `UTC` always has a timezone offset of 0, by definition.

The timezone `Pacific/Honolulu` always has a timezone offset of -600 because Hawaii does not observe DST.

The timezone `Australia/Melbourne` has a timezone offset of +660 approximately October through March (summer months in the southern hemisphere!) when DST is observed and +600 when standard time is observed the remainder of the year.

Tidepool's data model includes a calculated (via BtUTC) timezone offset in `timezoneOffset`.

- - - - -

## Unix time

Unix time is a particularly machine-friendly method for representing a datetime, defined as the number of seconds that have elapsed since midnight on January 1st, 1970.

Unix time is also sometimes referred to as "POSIX time" or "epoch time."

#### examples

`0` is the Unix time representing midnight on January 1st, 1970. `1495760648` is the Unix time representing the time just before I typed *this*.

We don't use Unix times at Tidepool. We use hammertimes instead.

- - - - -

## UTC

Coordinated Universal Time (UTC) is the primary date & time standard by which planet Earth regulates clocks and time.

UTC does not observe DST, and it is the successor to Greenwich Mean Time (GMT), which is no longer a functioning global standard. In the ISO 8601 standard, `Z` (for "Zulu", from the [radio alphabet](https://en.wikipedia.org/wiki/Spelling_alphabet 'Wikipedia: Spelling alphabet')) is a shorthand for represented the timezone offset of UTC (i.e., `+00:00`).

####

`2017-05-25T23:48:52.000Z` is an ISO 8601-formatted UTC datetime.

In Tidepool's data model, all `time` fields are in UTC.

- - - - -

[^a]: a.k.a. @jebeck's very favorite page on the whole of the Internet to link to.
