## Recommended[^a] Reading

I[^b] first came across the term "hypermodularization" in an article by Nicolás Bevacqua of the excellent [ponyfoo.com](https://ponyfoo.com/ 'Pony Foo'). The article was ["The Controversial State of JavaScript Tooling"](https://ponyfoo.com/articles/controversial-state-of-javascript-tooling 'PonyFoo: The Controversial State of JavaScript Tooling'), and it was one of many flying around in early 2016 in the wake of a particular (and rather offensive, due to *ad hominem* attacks directed against the [Babel](https://babeljs.io/ 'Babel') maintainers) Medium article[^c].

So, interestingly, the article where I first came across the term was *not* an article trying to *sell* hypermodularization or discuss it in particular. It was an article about current practices in the general world of JavaScript web development: what's good and what's working well as well as what's bad and what's not. The article *is* an opinion piece, however, and it does advocate hypermodularization *at the right level*. The whole article is recommended reading, but to pull out just the conclusion as a preview, this is it:

> Write wrappers and intermediate libraries that live outside of your application core while consuming hypermodules. Keep your opinions to those intermediate libraries, while keeping hypermodules discrete. In this sense, you could think of an hypermodule as `lodash/function/bind` and an intermediate library as `lodash`. I use `lodash` as an example because it’s one of the best representations out there today of what constitutes an hypermodular library. One that has hundreds of components, but **opinions are not one of them**.

> An ecosystem that’s founded on intermediate libraries on top of hypermodules could fare much better [than what's in existence today as the JavaScript ecosystem---JEB].

I believe that what's true of the JavaScript ecosystem applies micro-cosmically to the Tidepool ecosystem. As we move forward, and *especially* as we start to think about supporting third-party developers and various interests in the open-source community, we want well-tested, un-opinionated hypermodules at the lowest level that are easy (for us and for anyone) to drop in or wrap. And we will want to wrap these hypermodules in our opinions and for our specific use cases in an application like blip. Maybe we'll think our opinions are good enough and our use cases general enough that we'll provide our wrapper as an intermediate library for others; maybe we won't (i.e., the wrapping will remain as part of one of our projects, not as an independent project).

[^a]: Recommended by *whom*, you ask? Recommended by @jebeck, author of this section of "documentation" (really discussion notes).

[^b]: Yep, still @jebeck. Hiiiii.

[^c]: Link redacted due to the offensive content. Everything good that came out of the article came out of *reactions* to it, the most relevant of which **is** linked here.
