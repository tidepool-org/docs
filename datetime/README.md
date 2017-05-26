## @jebeck's guide to handling datetimes at Tidepool

**tl;dr**: Handling datetimes in the modern world is hard. Like, *really freaking hard*. 😓

What are some of the things that make it hard? A short list: DST, frequent cross-timezone travel, and technology like smart phones and browsers operating in an always locale-aware manner. This is @jebeck's handy guide to the most common situations at Tidepool where you'll have to handle datetimes and the best strategies for doing so.

### Prerequisites

We need to start with the same vocabulary, as some terms are similar but the differences between them can be *crucial*. Before continuing with this guide, review the following terms in the [glossary](../GLOSSARY.md):

- BtUTC
- calendar date
- clock time
- datetime
- DST
- hammertime
- ISO 8601
- timezone
- timezone offset
- Unix time
- UTC

In addition to this background vocabulary, here are a few readings and videos that are also well worth study, though maybe not all at once:

- [Falsehoods programmers believe about time](http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time 'Falsehoods programmers believe about time')[^a]

<!--TODO: dig up that one video TS linked me to ages ago; perhaps he remembers it?-->

### The guide(s)

The first key takeaway is that there is not *one* guide to datetimes at Tidepool. There are three: three distinct guides for three quite distinct situations. Or really, three distinct types of datetimes that are encountered regularly in the Tidepool codebase. So the first question a Tidepool engineer should ask themself is, "What kind of datetime am I trying to manipulate?" Then look at the appropriate guide below:

- [guide to handling diabetes data](./diabetes-data.md): This is the *epic* one, naturally given Tidepool's mission. If you are dealing with a `time` or a `deviceTime` from a datum conforming to the Tidepool data model, this is the guide for you!
- [guide to handling browser-local datetimes in the present moment (i.e., `now`s)](./now.md): If you're trying to manipulate the datetime representing a user's action in the present moment—that is, a "now"—then this is the guide you want.
- [guide to handling user-significant calendar dates](./user-dates.md): If you're working with any calendar date stored in the user's profile (as of May 2017, these are `birthday` and `diagnosisDate`), look at this guide.

[^a]: Hat tip to @darinkrauss for linking this one in the Tidepool Slack!
