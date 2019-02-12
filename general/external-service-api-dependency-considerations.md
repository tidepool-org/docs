# Tidepool External Service API Dependency Considerations

If you are thinking about adding a new external service API dependency to a Tidepool project, please give serious thought to the following considerations before committing to the dependency. There are few hard-and-fast rules, except perhaps the security and privacy requirements, so use your best judgement while taking these considerations into account. When in doubt, pull in another engineer or engineering manager to help with the decision.

This document applies mainly to the service itself, and the APIs it exposes for our use. If that service API is accessed through vendor-provided SDK, you may also want to keep in mind the related considerations for external dependencies as code. Those are covered in [Tidepool External Code Dependency Considerations].

You should repeat this process for any additional sub-dependencies that a service API introduces. In other words, other services that are used through calls to service API.

## Common Considerations

1. Are there reasonable alternatives? Including in-house? (build vs. buy/license)
2. Review the SLA
    1. What is their uptime promise?
    2. What is their escalation process?
    3. Is it supported 24/7 globally?
    4. Do they have an uptime dashboard?
    5. What is their EOL policy for legacy APIs?
3. Is the API available in all the geographies we need?
    1. Is the performance adequate from other geographies?
    2. What is their DR plan?
    3. Do they have automatic failover to another geography?
    4. If the service stores data persistently, is it subject to [GDPR] and similar laws?
4. How will it affect us if the service suffers a prolonged downtime? This could be outside of their control ([AWS us-east-1 outage](https://www.geekwire.com/2018/widespread-outage-amazon-web-services-u-s-east-region-takes-alexa-atlassian-developer-tools/))
5. Is the API available for different development environments? (dev, stg, prod, …). Are they wholly separate or mixed (e.g. user/account IDs)? Are the non-production environments sufficiently representative of performance etc. of the production environment. Do you have a clear understanding of how they differ?
6. Is the API rate limited? Does that rate meet our current and forecast needs?
7. Does the API support bulk operations? Are they needed by our use-cases?
8. Is there an SDK available in the language(s) we use? Do we need to write our own? Is the wire protocol tolerable (JSON, XML, …)?
9. Is their code open source? Can you look at it and use the same considerations as [Tidepool External Code Dependency Considerations]? Is it possible to install it locally and we manage it? Does that make sense?
10. What plan/consideration should be made if it were to go away overnight? How likely is that?
11. What does their funding, business model, expected longevity look like?

## Security & Privacy Specific Considerations

1. How is authentication and authorization handled? Whole service vs. per-user?
2. Is all traffic secured with TLS/SSL? Are their certificates up-to-date?
3. Will it be handling any [PII]/[PHI] data in transit or at rest? Do they comply with [HIPAA] requirements? Will they sign a [BAA], if necessary? If in doubt, consult Howard and Brandon.
4. What sort of security record does the code or project have? Do they have any outstanding security reports? What is the impact of previously reported security and other types of bugs?
    1. [https://cve.mitre.org/cve/search_cve_list.html](https://cve.mitre.org/cve/search_cve_list.html)
    2. [https://www.sourceclear.com/vulnerability-database/search](https://www.sourceclear.com/vulnerability-database/search#_)
    3. [https://www.openhub.net/](https://www.openhub.net/)
5. Do we plan to store any Tidepool or customer data there long-term?
    1. Data encrypted at rest?
    2. What is the plan for purging (account deletion, [GDPR] purge requests, …)

## Conclusion

Finally, please consider this document to be evergreen. If you think of
additional considerations that may be useful, please feel free to add
them above into the appropriate list and submit a Pull Request.

[Tidepool External Code Dependency Considerations]: ./external-code-dependency-considerations.md
[HIPAA]: https://www.hhs.gov/hipaa/index.html
[BAA]: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
[PII]: https://en.wikipedia.org/wiki/Personally_identifiable_information
[PHI]: https://en.wikipedia.org/wiki/Protected_health_information
[GDPR]: https://eugdpr.org/
