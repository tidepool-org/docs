## Pros & Cons of Hypermodularization

At Tidepool, on the server side we started out with the equivalent of hypermodularization—that is, a microservices architecture. We are currently moving away from this architecture for reasons of simplicity and efficiency (and to focus less on architecture and more on our unique offering: ingesting and handling diabetes data and managing the data sharing relationships between people with diabetes, their support networks and caregivers (including clinicians of various stripes), and researchers).

We[^a] mention the transition away from "hypermodularization" on the server side of things at Tidepool because it presents a good opportunity to discuss the advantages and disadvantages of hypermodularization, some of which involve greater pain or greater benefit on the back end or on the front end. The analogy between the microservices architecture on the server side and the type of hypermodularization we have [defined through examples](what-is-hypermodular.md) is not perfect, so have patience and keep an open mind.

### Pros

#### Clean separation of concerns

This should be a fairly self-explanatory, but I think it actually manifests in a somewhat different—and potentially more alluring—way on the front end.

Clean separation of concerns is essentially the motivation for *all* modularization of code. It's the natural result of following the Unix philosophy: "Do one thing and do it well."[^b]

On the "modern" front end with a complex single-page application, we have the opportunity to *really* separate our concerns, not just by task or subject matter, but by *kind*. A modern front end application may have many different areas with quite different *kinds* of concerns, for example:

- an API communication layer, consisting mostly of asynchronous functions
- a data processing layer that does something with data passed to it from the API layer
- a layer that manages the state of the application, includes synchronizing with possibly many other layers and interfaces (API layer, data processing layer, client-side routing layer)
- a "smart" interface layer that renders UI elements and logic for pieces of the application under user control
- a "pure" rendering layer that renders non-interactive UI elements

These different *kinds* of concerns on the front end often have vastly different requirements. For example, the kind of tooling required to instrument tests for a small data processing module will be far different (and simpler) than the tooling required to test the rendering of a set of React components in real browsers.

#### DRYness

Hypermodularized code *can* be DRYer[^c], but it isn't always—see [boilerplate code](#boilerplate-code-antidryness) below.

The key to whether hypermodularization results in DRYness or anti-DRYness almost certainly comes down to how the modules integrate and how well designed their interfaces are.[^d]

#### High-speed iteration

Hypermodularization *should* lead to greater speed of development, though this seems in practice to be heavily contingent on the counterbalancing complexity costs (see [below](#cons)). The reasons for increased development velocity with hypermodularization are several:

- It is easy to have many hands on the code in parallel without engendering conflict.
- Each module is small and relatively easy to understand; what complexity there is resides chiefly in the interfaces, not in the components themselves.
- Some modules in the code will be accessible even to those with limited skillsets (including designers with limited knowledge of HTML/CSS and no JavaScript, for example), thereby allowing *more* hands on the code.
- As long as there are no new interface requirements, experimental modules (possibly short-lived) are easy to spin up and play with.
- Deployment is still (relatively) simple, via a "monolithic" build into a single bundle.

#### The Interface is King :crown:

Hypermodularization naturally forces complexity to the edges—the places where small modules connect and interact with each other. This is a benefit in some ways: testing is easier, and debugging should be easier. Consider: if you are connecting a couple of small modules, both of which are quite simple and well-tested on their own, then you have a problem connecting them, where are you going to look (or write tests) to find the issue? The only option is the interface layer.

### Cons

#### Boilerplate code (anti-DRYness)

This disadvantage applies most when each hypermodular component project runs in parallel to the others. *If*, as there is/was with our server-side microservices, there is some boilerplate that each component must include in order to run alongside the others, but it's not boilerplate that's common *enough* that *it* can be extracted as a small module/common dependency, then this boilerplate is another large source of developer pain.

In certain circumstances, this boilerplate can even become a large source of *risk*—if, for example, a problem is discovered that requires changing the boilerplate in every component module in order to fix the issue.

#### Common dependencies

*If* your hypermodular components are each contained within their own repository and manage their own dependencies, any common dependencies across components can be *very tedious* and/or error-prone to update.

At Tidepool, we keep our dependencies "locked-down" to the patch level and bringing in dependency updates can end up being a large project, especially when it might mean updating and testing large numbers of components.

#### Complex deployment

With a microservices architecture on the server, each service is deployed (and potentially scaled) individually. This adds a considerable complexity cost.

On the front end, however, a hypermodularized architecture only really adds *length* (not complexity) to one's `package.json` file and perhaps a bit of complexity to one's build process if (small) size of the bundled application and quick download times are a high priority and some kind of "tree-shaking" via (a tool like) [`rollup`](https://www.npmjs.com/package/rollup 'npm: rollup') is required.

#### Complex local(host) setup for developers

Really goes hand-in-hand with "complex deployment." Currently even front end developers at Tidepool often spin up the entire platform (over a half dozen and maybe even closer to a dozen services, as well as mongo) in order to work locally with full feature support.

The front end equivalent—*if* hypermodularization is accomplished through having many modules in *separate* repositories—is having to do a *lot* of `npm link`-ing (and suffering through the resulting glitches relating to [webpack](https://webpack.github.io/ 'webpack') bundling).

#### The Interface is a Nightmare :scream:

A corollary of [the above](#the-interface-is-king-). The flip side of the fact that complexity migrates to the interfaces between small modules is the fact that *complexity migrates to the interfaces between small modules*. The interface becomes a potential single point of failure, and the devil is in the details. If the interface isn't well-designed to begin with or becomes difficult to work with due to changing requirements imposed on a design that wasn't flexible enough to accommodate the changes, then the interface can become a horror and a developer's worst enemy.[^e]

#### Many dependencies

The more *external* dependencies you have, the more risk you carry in terms of falling prey to something like the [left-pad nightmare of 2016](http://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm 'npm blog: kik, left-pad, and npm'). If your "hypermodularization" is more "hyper" internally, however, this is not an issue.

[^a]: Haven't you figured out yet who wrote this section? (@jebeck, again.)

[^b]: ([source](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well 'Wikipedia: Unix philosophy'))

[^c]: [**D**on't **R**epeat **Y**ourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself 'Wikipedia: Don't Repeat Yourself')

[^d]: @jebeck has a preliminary hunch that pieces designed to operate in *parallel* (though with some communication between them) may be harder to make DRY than pieces that are designed to operate as a whole, hierarchically with communication in a single direction.

[^e]: To digress into a bit of editorial: this is essentially what happened with the interface for building charts used by tideline's one-day and two-week views; it wasn't flexible or well-designed enough to adapt to new requirements posed by things like the trends view, the basics view, or even to the requirement of changing how the daily view renders to enable two-finger trackpad scrolling.
