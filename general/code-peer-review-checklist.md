# Tidepool Code Peer Review Checklists

In order to consistently ensure high quality code, a peer review of all code changes is required. It is recommended that the developer actually perform a code review of their own code **before** submitting it for peer review. This will hopefully reduce the amount of time and effort spent by the reviewer by catching common issues beforehand.

This document is intended as guidance for engineers. It is not a Standard Operating Procedure (SOP).

## Common Checklist

1. Does the code complete the feature requirements? Does it match the Jira issue?
2. Is the feature branch up-to-date with the `develop` branch?
3. Does the continuous integration build (e.g. Travis, CircleCI) pass? Do any of the non-required continuous integration steps fail? If so, why? Can these be fixed? If not, why not?
4. If any external dependencies were added:
    1. Review the results of the [Tidepool External Dependency Considerations] for each new dependency and sub-dependency.
    2. Do all of the new dependencies and sub-dependencies use an acceptable license?
    3. Are the new dependencies and sub-dependencies properly versioned and/or vendored to allow for future repeatable builds?
5. If any external dependencies were updated:
    1. Was each update explicit?
    2. Were any updates accidental or unintentional?
6. Does all new or updated code have a minimum level of test coverage?
7. Are critical edge cases properly tested?
8. Is overall test coverage at least the same or improved?
9. Visually review all new and updated code, with particular focus on the non-test code.
    1. Does it generally follow good industry practices, such as:
        1. [Separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns)
        2. [SOLID](https://en.wikipedia.org/wiki/SOLID)
        3. [Single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) - a class should have only a single responsibility (i.e. only changes to one part of the software's specification should be able to affect the specification of the class)
        4. [Open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle) - software entities should be open for extension, but closed for modification
        5. [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle) - objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program
        6. [Interface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) - many client-specific interfaces are better than one general-purpose interface
        7. [Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) - one should depend upon abstractions, **not** concretions
        8. [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) - Don't Repeat Yourself
        9. [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) - You Ain’t Gonna Need It (did you gold plate it?)
        10. [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) - minimum needed to get the job done
    2. Does it conform to the latest best-practices for that repository, language, and/or framework (e.g. format, lint, import)?
        1. Naming conventions (CamelCase, UPPERCASE, etc.)?
        2. Spaces vs. tabs and indentation, as appropriate for the repository.
    3. Is it reasonably self-documenting?
        1. Variable names should be descriptive. Use of single-letter variable names are typically only valid for loop index variable names.
    4. For complex algorithms, is the algorithm documented through more than just the code (i.e. comments)?
    5. Are all returned errors and exceptions checked?
    6. Are all returned errors and exceptions properly handled? Depending upon the situation this could mean propagating the error to the caller and/or reporting the error to the log.
        1. Are all resources properly disposed even when an error occurs (e.g. files actually closed if there is a failure while reading, temporary files deleted).
    7. Do all switch statements handle the default case properly?
    8. Do all for loops correctly manage memory and other resources?
        1. Resources opened within a loop usually need to be confirmed closed in the loop.
        2. Memory allocations within a loop may not be garbage collected until well after the loop completes.
        3. Consider unrolling or breaking up problematic loops.
    9. If any code was copy-pasted:
        1. Why? Could the code have been reused instead with a little effort?
        2. If not, were any subtle bugs introduced with the copy-paste?
    10. Is it more complicated than it needs to be? Can it be made simpler? Easier to read or understand?
    11. Does it use framework features wherever possible? No sense in reimplementing standard library features.
    12. Are there *extra* features that *might get used someday*? If so, consider removing the extra, unused code.
    13. Are there any functions or methods that are particularly long (e.g. more than 30 lines or so)? Should they be broken down into separate, more focused functions that are easier to understand and test?
    14. Is there any newly comment-out code? Any code not being used should be removed. Under certain uncommon circumstances it is okay to leave commented-out code, but there should be detailed documentation before the block that describes why it is commented out and yet still remains in the code.
    15. Are there any special values or hard-coded values? If so, they should probably be constants or enums, especially if shared across modules/interfaces/API boundaries/etc.
    16. If the code relies on external resources (e.g. another service), does it properly handle if the external resource is:
        1. Not available
        2. Timed out
        3. Appears to accept the request, but does not return a response
        4. Returns an unexpected response
    17. Is all *user* input validated before use? (Where *user* means any input coming from outside of the code, not just a physical person.)
10. Was any technical debt added?
    1. If so, why? It should be a really, really good reason why it was not addressed
    2. It should be clearly documented where the tech debt was added.
    3. A new Jira issue should be created detailing the tech debt and specifically what it will take to remove that debt.
11. Are there any `TODO`, `FIXME` or similar comments? If so, why? `TODO` is just a form of tech debt, so see above.
12. Was the Pull Request focused? It is best if the Pull Request is focused to one new feature to make it easier to review.
13. How many commits are involved in the Pull Request? If there is just one commit for a large number of unrelated changes this may point to lack of focus for the Pull Request. If there are a large number of commits, seriously consider rebasing down to a reasonable number. Strive for one commit per logical unit of work, but a feature may include multiple logical units of work. A logical unit of work is the smallest amount of work that can be completed and still have a functional project.
    1. For example, if I need to do a refactor in order to implement a new feature, it would make sense to have one commit for the refactor (and still have a functioning project), one commit for just the new feature after the refactor, and one overall pull request for both commits.
14. If there were UX changes, was a design review completed and approved?
15. Was any pertinent documentation (internal and/or external) properly updated to reflect the code changes?
16. Is all [PII]/[PHI] communication and storage properly encrypted?
17. Was there any security review?

## Specific Checklists

Here are some items to consider that are specific to a particular language and/or framework:

### Service Specific Checklist

1. If there were API or data model changes, was the documentation updated?
2. If there was a new API added:
    1. Does it require TLS/SSL?
    2. Does it properly check the request for valid authentication and authorization? Does it properly return an error, if either fail?
3. Does it belong in that service?
4. Does the code use local resources (e.g. disk)? If so, will it still work if the next request goes to a different service?
5. Is the code scalable? Was it tested with more than one node?
6. Do new database queries correctly use existing indexes? Do they require new indexes? What is the performance impact if a query is used or not?
7. Do environment specific settings get loaded properly via the standard configuration mechanism for the service?

### Golang Specific Checklist

1. All external dependencies **must** be vendored using the standard Tidepool Golang dependency management tool (currently `go dep`).
2. Best practices for Golang use "early exit on error". Are all errors checked and does the error case return immediately while the success case continues on with the remainder of the function?
3. Use interfaces wherever possible, but when creating a new struct, return a concrete type, not an interface.

### Node.js Specific Checklist

1. All dependencies must specify an exact version (i.e. `1.2.3`). Note that using any other form (e.g. `1.2.x`) could yield non-reproducible builds if the lock file is regenerated.
2. Was a new dependency added? If so, was the `yarn.lock` file updated to match?
3. If the `yarn.lock` file was updated, why? Were the dependency updates reviewed and tested?

### React Specific Checklist

1. Are React packages used/added by this change available in React Native? For both iOS and Android?

### iOS Specific Checklist

1. Use guards to reduce conditional logic in the main body of a function.
2. Force unwrapping optionals is usually a bad idea. Guards or optional chaining is often a better solution.
3. Prefer `let` over `var`.
4. Make sure observer registrations are paired with unregistrations
5. Watch for retain cycles. Use unowned or weak references to break them.
6. Review for concurrency issues. Data accessed by multiple threads should be protected by a lock
7. Delegate methods should have the delegating object included in the method signature.

## Conclusion

Finally, please consider this document to be evergreen. If you think of additional checklist items that may be useful, please feel free to add them above into the appropriate list and submit as Pull Request.

If you really want to go crazy with a peer review, check out the [OWASP Code Review Guide].

Great read on how to keep PR reviews supportive and constructive for both the reviewer and reviewee: [Unlearning toxic behaviors in a code review culture](https://medium.freecodecamp.org/unlearning-toxic-behaviors-in-a-code-review-culture-b7c295452a3c).

[Tidepool External Dependency Considerations]: ExternalDependencyConsiderations.md
[PII]: https://en.wikipedia.org/wiki/Personally_identifiable_information
[PHI]: https://en.wikipedia.org/wiki/Protected_health_information
[OWASP Code Review Guide]: https://www.owasp.org/images/2/2e/OWASP_Code_Review_Guide-V1_1.pdf
