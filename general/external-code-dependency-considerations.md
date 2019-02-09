# Tidepool External Code Dependency Considerations

If you are thinking about adding a new external code dependency to a Tidepool project, please give serious thought to the following considerations before committing to the dependency. There are few hard-and-fast rules, except perhaps the license requirement, so use your best judgement while taking these considerations into account. When in doubt, pull in another engineer or engineering manager to help with the decision.

This document applies to code. If that code depends on a service API for its operation (e.g. it is a vendor-provided SDK), you may also want to keep in mind the related considerations for service APIs. Those are covered in [Tidepool External Service API Dependency Considerations].

You should repeat this process for any additional sub-dependencies that a dependency may pull in.

## Common Considerations

1. What license does the dependency use? The MIT, BSD, and Apache licenses are preferred and may be required depending on how you will be using the dependency. If there is any question, get approval from Howard.
2. Are there any reasonable alternatives? Spend a few minutes searching for alternatives. If there are alternatives, then use this process with those, too, as a comparison.
3. How much code is actually going to be used from the dependency? If only a small amount of code compared to the overall size of the dependency, then consider finding an alternative or just implementing it locally. Even consider copy/paste if the license allows for it, but remember to give credit to the original work.
4. What sub-dependencies does that dependency have? How much extra, unused stuff is coming along for the ride? Apply this process to those sub-dependencies.
5. What sort of security record does the code or project have? Do they have any outstanding security reports? What is the impact of previously reported security and other types of bugs?
  1. [https://cve.mitre.org/cve/search_cve_list.html](https://cve.mitre.org/cve/search_cve_list.html)
  2. [https://www.sourceclear.com/vulnerability-database/search](https://www.sourceclear.com/vulnerability-database/search)
  3. https://www.openhub.net/
6. How active is the community for the dependency?
  1. How many active contributors? Recent turnover? (See [event-stream](https://github.com/dominictarr/event-stream/issues/116) incident)
  2. What is the frequency and recentness of commits?
  3. How many open issues are there? What is the impact of these open issues? Do they represent significant problems or just minor problems and feature requests?
  4. How frequently are issues closed? What is the responsiveness? Why were they closed? Were they actually fixed or were they just closed for lack of progress?
  5. Review GitHub Insights.
7. How much interest is there from the outside community (e.g. stars, watches, forks)?
  1. Is there any fork that may be more appropriate? Some forks are more active, include additional bug fixes, and are an overall better choice, particularly if work on the primary project has stalled. Repeat this process for any forks that might be viable.
8. Review the code.
  1. Consider applying all or part of the [Tidepool Code Peer Review Checklist] against the dependency.
  2. What does the code look like (readability, understandability, etc.)?
  3. Does the code conform to any coding standards or best practices for the language?
  4. What documentation is available?
    1. For external use?
    2. For internal development and maintenance?
  5. What test coverage does the dependency have?
    1. Run the tests. Do the tests pass?
    2. Are they executed automatically on each PR/commit?
  6. What update strategy does the dependency use for backwards compatibility? Is anything documented? If not, review the change log or actual code changes between previous versions.
9. How will we "lock" a particular reviewed version of the dependency (and any sub-dependencies) to ensure we get an exact reproducible build (even years later)? Can/should the dependencies be vendored?
10. Consider the impact of the dependency to our [HIPAA] compliance requirements

## Specific Considerations

Here are some items to consider that are specific to a particular language and/or
framework:

### Golang Specific Considerations

1. What version(s) of Golang has the dependency been built and tested against? If it does not explicitly support the latest version the Tidepool repository uses, double check the Golang release notes for that version to see if are any potential conflicts.
2. Does the dependency use the standard Golang formatting and linting tools?
3. All external dependencies *must* be vendored using the standard Tidepool Golang dependency management tool. The reasons for this are:
  1. The go tool chain does not guarantee that a dependency will be available in the future. For example, a developer can delete a repo. Without a store of all published versions, the dependency would disappear.
  2. The dependencies must be available at build time. Vendoring simply stores those dependencies in the local repo. At present this consists of over 500K lines of Go code. Since space is basically free, this is not a big concern.

### Node.js Specific Considerations

1. Are you using a `yarn.lock` or a `package-lock.json` for you repo to ensure that sub-dependencies are locked down?

2. Only use fixed versions of dependencies, i.e., don’t use version ranges (^,\~)

### React Specific Considerations

None

### iOS Specific Considerations

None

## Conclusion

Finally, please consider this document to be evergreen. If you think of
additional considerations that may be useful, please feel free to add
them above into the appropriate list and submit a Pull Request.

[Tidepool Code Peer Review Checklist]: ./code-peer-review-checklist.md
[Tidepool External Service API Dependency Considerations]: ./external-service-api-dependency-considerations.md
[HIPAA]: https://www.hhs.gov/hipaa/index.html
