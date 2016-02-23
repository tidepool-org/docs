## `git` and GitHub usage at Tidepool

At Tidepool, we use `git` for version control, with repositories hosted [on GitHub](https://github.com/tidepool-org/ 'Tidepool on GitHub').

This document describes our guidelines for `git` and GitHub usage.

### Branches and pull requests

In all application and library repositories, we use the `master` branch for production code. Production _releases_ are tied to git [tags](#tags). (In our ops system, it is only possible to build and deploy an app or service from a tag.)

When making a change in any repository (whether developing a new feature or fixing a bug), our convention is to create a new branch with the name `<GitHub username>/<short description>` - e.g., `jebeck/new-signup-flow` or `jebeck/bugfix-form-validation`. The backend team occasionally uses sprint-based branch names - e.g., `jh-bate/sprint25` or `jh-bate/sprint25-clinic-roles`.

All significant changes should be made through a pull request that is approved by another Tidepool developer through a process of code review. We often use LGTM "looks good to me" as our approval sign-off at the conclusion of a review and/or one or more rounds of feedback and changes. One of the following (or any similarly unambiguous) emoji is also acceptable as sign-off: :ship: (:ship:), :thumbsup: (:thumbsup:), or the GitHub ["ship it squirrel"](https://www.quora.com/GitHub/What-is-the-significance-of-the-Ship-It-squirrel 'Quora: Ship It Squirrel') :shipit: (:shipit:,:squirrel:).

What constitutes a *significant* change? In essence, it's anything that could potentially affect the user experience (though perhaps indirectly). It's probably easier, however, to just list the few things that are *not* significant changes:

- updates to dev-only dependencies
- changes to dev-only tooling (dev tools like the dev server for client-side apps, test runners, CI configuration)
- changes to the README or documentation

Changes such as these that are *not* significant should still be made through a pull request, but the developer opening the pull request may merge the PR without code review and sign-off. Opening a pull request in these cases serves to make other developers aware of the changes made. The developer opening the pull request may also wish to leave the pull request open for a small amount of time for potential commenting and/or requests for changes. Our convention is to @-mention any other developers we would like to notify of the changes and give a period of time that the pull request will be open for comment before merge. For example:

> @gniezen, does this look OK to you? Merging in twenty-four hours if no comment 😎

Note that the following things, which may *seem* insignificant, are subject to code review (and QA):

- updates to production dependencies
- changes to production build tooling
- changes to the production static server (for client apps like blip)

Sometimes we open a pull request before code is ready for review if we want another developer to take a look at the code progress so far. (GitHub's interface for viewing diffs being one of the easiest ways to do this.) In such cases, the PR should be clearly marked as a work-in-progress - e.g., **WIP. Don't merge!** If you open a WIP pull request, please remember to *remove* the WIP warning language When the PR *is* ready for review and merge.

#### Continuous Integration

In the vast majority of our app, service, and library repositories, if not all of them, we use the [Travis](https://travis-ci.org/ 'Travis CI') continuous integration service, integrated with GitHub, to run linting and tests on every push and pull request. When you open a pull request, if Travis reports a failure, your first task will be to fix whatever caused it to fail (linting, tests, or both). Anyone you've asked to review a PR may decline to do so until Travis is happy. Conversely, if someone has asked you to review a PR and Travis is in a sad/angry/&c state, you may decline to begin reviewing until whatever Travis is choking on has been fixed.

You should also wait for Travis to report back happily on a tag push before deploying the tag. (Exception: it's a known non-RC, tagged as such, and only being deployed to the dev environment.)

#### Merging

Our conventions for merging pull requests vary - that is, whether the code reviewer does it, or whether the code reviewer signs off and then waits for the PR opener to do it. The strategy employed in any given situation depends on how many things are in flight in the repository, the priority of sprint goals for production releases, etc. Communicate clearly with your teammates about who's going to merge and when.

After a pull request is merged, the branch should be deleted. (We also periodically clean up stale branches in each repository, and it is here that the developer-based branch naming convention is particularly helpful because it ensures that everyone knows who to ask about whether a stale branch needs to be kept or can be deleted.)

### Rebasing

We follow the general guideline of "don't rewrite history." Rebasing to reorder, squash, or otherwise fix commits locally before a branch or a set of commits has been pushed upstream is perfectly acceptable, but once commits are upstream you shouldn't mess with them (by rebasing or by any other means).

If you have a strong need to rebase a branch that has already been pushed upstream, a good strategy is to create a new branch with a "version number" increment. For example, from `darinkrauss/custodial-accounts` create `darinkrauss/custodial-accounts.2`, rebase as necessary *before* pushing `darinkrauss/custodial-accounts.2` upstream, then delete `darinkrauss/custodial-accounts` from the upstream repo.

### Tags

We "cut" tags in two situations, the first far more common than the second:

1. when we think we have a release candidate of an app or service
1. when we know we *don't* have a release candidate of an app or service, but we want to deploy what we have so far to demo it or make it available for developer use

In the first situation, we create the tag after merging the pull request(s) for the new feature(s) or fix(es) to master. In the second situation, we create the tag on the working feature branch. We use [semantic versioning](http://semver.org/ 'semver.org') for the first type of tag (the uploader is an exception, since the Chrome store does not support patch-level version increments), and most of us use the tool [mversion](https://github.com/mikaelbr/mversion 'mversion') to create these tags in a consistent way. You can bump the minor version for an uploader release, for example, with the command `mversion minor -m` and this will bump the version in *both* the `package.json` and the `manifest.json` (for the Chrome store), as well as commit those changes, named the commit with the new version number, and create the git tag. We typically distinguish the second type of tag from the first to make it very clear that the "demo" tag is not a release candidate, either by adding a post-fix (e.g., `v0.12.0-alpha`) or by using a named tag instead of a semver-based tag (e.g., `clinic-accts-beta`).

We also - very rarely - deploy from a tag made on a "hotfix" branch. This happens when an urgent bugfix needs to made, but changes have already been merged into master that are not ready for release. In such situations, we branch off the previous actual production release tag (e.g., `v0.5.0`), fix the bug on the branch and tag *on the branch* (bumping the patch number from the previous release and adding a post-fix to identify the tag as a "hotfix" tag: `v0.5.1-hotfix`). After the hotfix release, we may choose to merge the bugfix branch into master, or we may re-implement or copy over the bugfix changes in another way, depending on how the bugfix fits (or doesn't) with the code on master.

When you cut a tag, you should fill out the release notes for the tag via GitHub's interface for this at `https://github.com/tidepool-org/[repo]/releases/new`. Look at prior releases for an idea of how verbose these usually are (not very). If the tag is not a release candidate, check the "This is a pre-release" box.
