At Tidepool, we are using [GitBook](https://www.gitbook.com/) for documentation across our client-side apps and here in this repository for "no man's land" docs without a proper home in another repo. GitBook is a tool for generating a static documentation website (with built-in search) from a set of Markdown files (which are handily thus natively renderable by the GitHub online interface). We use [GitHub Pages](https://pages.github.com/) to publish the GitBook-generated static assets on the web at various paths under the base URL of our developer microsite at [http://developer.tidepool.org/](http://developer.tidepool.org/). If you need to add or update the docs in this repo or in another repo that employs GitBook for publishing documentation, follow the instructions below.

Jump to...
- [Setting up your repo to work on docs](#setting-up-your-repo-to-work-on-docs)
- [Adding or updating docs](#adding-or-updating-docs)
- [Publishing docs](#publishing-docs)

### Setting up your repo to work on docs

When we at Tidepool first set up our docs workflow using GitBook, there was only one way to set up [GitHub Pages](https://pages.github.com/ 'GitHub Pages') for a project: putting the static assets (HTML, CSS, etc.) you wanted GitHub to serve as your GitHub Pages site on a branch in your repository named `gh-pages`. This method of building a GitHub Pages site still exists, and it's what we still use at Tidepool[^a]. Every time we update the static assets on the `gh-pages` branch and push to the GitHub remote repository, our project page is updated to serve the new static files.

The workflow for managing one's static assets when the method of publishing is the `gh-pages` branch is a little unintuitive since the code on the `gh-pages` branch (a bunch of HTML & CSS files) has very little relation (in most cases) to the code on the `master` and other branches in the repository. We follow the workflow originally recommended by GitHub, which involves creating an embedded separate clone of the repository with *only* the `gh-pages` branch. Using a separate clone to manage the `gh-pages` branch makes everything easier and less confusing/dangerous when there is *zero* code shared between the `gh-pages` branch and `master` (or other branches). When you *embed* the separate clone inside your working repository—in our case, always with the directory name `web` (see step (1) in the setup instructions below)—this allows scripting tools to target the embedded clone/directory where the `gh-pages` branch is *always* checked out. In our case, every repository using GitBook contains the GitBook source Markdown files on `master` (often in a `docs/` directory but also in other locations as necessary). Then the `update-gh-pages.sh` build script included in each repository runs the GitBook build tool to generate the static HTML, CSS, and JavaScript files *in the embedded repository clone* so that the build result ends up on the `gh-pages` branch where it needs to be to "publish" the site updates by pushing the branch up to the GitHub remote.

To set up this workflow for yourself, follow these steps:

1. Clone another copy of this repository, nested inside your top-level `docs` directory, but give it the name 'web': `git clone git@github.com:tidepool-org/docs.git web`. (The optional final argument to `git clone` tells git to name the cloned directory something other than the default repo name.)
1. Navigate into the new clone with `cd web/`.
1. Checkout the gh-pages branch in the new clone with `git checkout gh-pages`.
1. Delete the `master` branch in the new clone since you will *only* be using this clone for managing the GitHub Pages code for the repo: `git branch -d master`.
1. Test that you can view the GitHub Pages site locally by serving up the site to yourself (e.g., with the Python one-liner `python -m SimpleHTTPServer`) and viewing it in a browser.

(Example given for this repository `docs`, but the same procedure is recommended for all Tidepool repos using GitBook, just substitute the repo name (e.g., `uploader`) for `docs` in the above instructions.)

When you want to make changes to docs, you will branch from `master` and edit the source Markdown file(s), then open a pull request against `master` as usual. After the pull request is merged, simply run the `update-gh-pages.sh` script and follow the instructions at the conclusion of the script to navigate into `web/` and check that the build was successful (e.g., step (5) above), that no non-HTML, CSS, or GitBook-related JavaScript files were incorrectly copied into the embedded clone[^b], and then commit the changes and push `gh-pages` to the GitHub remote to "publish" the changes to the site.

### Adding or updating docs

The [GitBook documentation](https://help.gitbook.com/) is a good, well-organized resource, but the basics are very simple and summarized here.

GitBook is a tool for compiling documentation from [Markdown](https://daringfireball.net/projects/markdown/) files. At Tidepool, thus far our only target for compilation is a static website that can be hosted on GitHub Pages, but GitBook offers other compilation targets, including PDF and e-reader formats.

At the top level, GitBook requires at minimum a `README.md`, which serves as the landing site for the documentation. (This `README.md` you are reading right now is thus the main page content for [http://developer.tidepool.org/docs/index.html](http://developer.tidepool.org/docs/index.html).) A `SUMMARY.md` is also required; the `SUMMARY.md` is where the docs are organized hierarchically, and one very nice thing about GitBook is that the hierarchy as described in the `SUMMARY.md` does *not* have to match the directory structure of the repository! If a Markdown file is *not* linked in the `SUMMARY.md`, it will **not** be compiled by GitBook (and may appear in your browser as plain Markdown), even if it is linked from another file that *is* linked in the `SUMMARY.md`. This issue of a missing link to a new file from the `SUMMARY.md` often surfaces with the error message "[some page] contains an hyperlink to resource outside spine [unlinked Markdown file]" during compilation. Basically, if you see that error, check the `SUMMARY.md` to make sure it includes everything it should.

Within each sub-directory of docs in your repository, it is conventional to include a `README.md` as the intro and table of contents for that sub-section of the documentation. In general, a typical GitBook structure (when one is matching directory structure to the `SUMMARY.md`, although this is not required) looks like the following:

```Markdown
# Summary

- [sandwich](sandwich/README.md)
   + [bacon](sandwich/bacon.md)
   + [lettuce](sandwich/lettuce.md)
   + [tomato](sandwich/tomato.md)

```

Every repository (including this one) that uses GitBook will include scripts for serving the docs to yourself locally (in order to work on them) and to compile the docs, copying the result into the embedded clone of the repository used to manage the `gh-pages` branch (see [Setting up your repo to work on docs](#setting-up-your-repo-to-work-on-docs) above).

To serve the docs to yourself locally at [http://localhost:4000/](http://localhost:4000/):

```bash
$ npm run serve-docs
```

(In this repository and [the data model docs repository](https://github.com/tidepool-org/data-model), the primary purpose of both of which is documentation, you can also serve the docs with `npm start`.)

To compile the docs to a static website and copy the result to the embedded clone of the repository in `web/`:

```bash
$ npm run build-docs
```

### Publishing docs

After building the docs with `npm run build-docs`, publishing the docs via GitHub Pages requires only a few additional steps:

1. First, navigate to the embedded clone of the repo used for managing the `gh-pages` branch with `cd web/`.
1. (If desired, preview the static site by serving it to yourself with a simple static server (e.g., the Python one-liner `python -m SimpleHTTPServer`).)
1. Check the changes with `git diff` or however you prefer to view changes. One thing to look out for here is irrelevant files that got synced - you may need to clear the changes inside `web/`, adjust the list of ignored files and directories in the `.bookignore` file, and rebuild the static site if files unnecessary for the static site (which should be just HTML files, outside of the CSS, JavaScript, and fonts provided under `gitbook/`) have been copied into `web/`.
1. Stage and commit the changes if you're happy with them.
1. Publish to GitHub Pages by pushing your new commit(s) to the upstream `gh-pages` branch: `git push origin gh-pages`.

[^a]: GitHub has since added a couple of far simpler methods, which are simply (a) including an `index.html` at the root level of your repository on `master` and choosing the appropriate repo setting to use that as the index for your GitHub Pages site or (b) including all static assets for your GitHub Pages site in a `docs/` directory on `master` (again choosing the appropriate repo setting to tell GitHub to serve the files in this directory as your static site). Eventually perhaps it will make sense for us to upgrade to the simpler `docs/` directory workflow, but at the moment with everything set up in the `gh-pages` branch workflow, investing the time to make the switch is not a high enough priority. (The `gh-pages` branch workflow also has the advantage of being somewhat cleaner when creating docs with GitBook since only the raw, source Markdown files are tracked on `master`, removing the potential for a mismatch between the source and compiled static assets, if both were tracked on `master`.)

[^b]: If you do find non-GitBook HTML, CSS, and JavaScript files in the changes in the embedded clone, you may need to update the [`.bookignore`](https://toolchain.gitbook.com/structure.html#ignore 'GitBook Toolchain Docs: Ignoring files & folders') file.
