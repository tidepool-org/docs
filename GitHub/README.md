## `git` and GitHub usage at Tidepool

At Tidepool, we use `git` for version control, with repositories hosted [on GitHub](https://github.com/tidepool-org/ 'Tidepool on GitHub').

This document describes our guidelines for `git` and GitHub usage.

### Branches and pull requests

In all application and library repositories, we use the `master` branch for production code. Production _releases_ are tied to git [tags](#tags).

When making a change in any repository (whether developing a new feature or fixing a bug), our convention is to create a new branch with the name `<GitHub username>/<short description>` - e.g., `jebeck/new-signup-flow` or `jebeck/bugfix-form-validation`. The backend team sometimes/always(?) uses sprint-based branch names - e.g., `darinkrauss/sprint25` or `darinkrauss/sprint25-clinic-roles`.

All significant changes should be made through a pull request that is approved by another Tidepool developer through a process of code review. We often use LGTM "looks good to me" as our approval sign-off at the conclusion of a review and/or one or more rounds of feedback and changes.

What constitutes a *significant* change? In essence, it's anything that could potentially affect the user experience (though perhaps indirectly). It's probably easier, however, to just list the few things that are *not* significant changes:

- updates to dev-only dependencies
- changes to dev-only tooling (dev tools like the dev server for client-side apps, test runners, CI configuration)
- changes to the README or documentation

Only changes such as these that are *not* significant may be pushed to master, and even then you may see some of us opening pull requests for such changes if we either wish to expose the changes to our peers for comment or just to prevent potential conflicts on master if a lot of things are currently in flight. Basically: when in doubt, open a pull request!

Note that the following things, which may *seem* insignificant, are subject to code review (and QA):

- updates to production dependencies
- changes to production build tooling
- changes to the production static server (for client apps like blip)

### Rebasing

We follow the general guideline of "don't rewrite history." Rebasing to reorder, squash, or otherwise fix commits locally before a branch or a set of commits has been pushed upstream is perfectly acceptable, but once commits are upstream you shouldn't mess with them (by rebasing or, really, by any other means).

The one exception to this is large, long-running feature branches. If you feel a need to clean up a large, long-running branch, the polite thing to do is to ask the other developer(s) you are collaborating with most closely on the work and/or the developer(s) who will be expected to review the code if they mind if you rebase and force push upstream. But again, this is the exception, not the rule.

### Tags

We "cut" tags in two situations, the first far more common than the second:

1. when we think we have a release candidate of an app or service
1. when we know we *don't* have a release candidate of an app or service, but we want to deploy what we have so far to demo it or make it available for developer use

In the first situation, we create the tag after merging the pull request(s) for the new feature(s) or fix(es) to master. In the second situation, we create the tag on the working feature branch. We use [semantic versioning](http://semver.org/ 'semver.org') for the first type of tag (the uploader is an exception, since the Chrome store does not support patch-level version increments), and most of us use the tool [mversion](https://github.com/mikaelbr/mversion 'mversion') to create these tags in a consistent way. You can bump the minor version for an uploader release, for example, with the command `mversion minor -m` and this will bump the version in *both* the `package.json` and the `manifest.json` (for the Chrome store), as well as commit those changes, named the commit with the new version number, and create the git tag. We typically distinguish the second type of tag from the first to make it very clear that the "demo" tag is not a release candidate, either by adding a post-fix (e.g., `v0.12.0-alpha`) or by using a named tag instead of a semver-based tag (e.g., `clinic-accts-beta`).

We also - very rarely - deploy from a tag made on a "hotfix" branch. This happens when an urgent bugfix needs to made, but changes have already been merged into master that are not ready for release. In such situations, we branch off the previous actual production release tag (e.g., `v0.5.0`), fix the bug on the branch and tag *on the branch* (bumping the patch number from the previous release and adding a post-fix to identify the tag as a "hotfix" tag: `v0.5.1-hotfix`). After the hotfix release, we may choose to merge the bugfix branch into master, or we may reimplement or copy over the bugfix changes in another way, depending on how the bugfix fits (or doesn't) with the code on master.
