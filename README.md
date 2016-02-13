# docs

This is our repository at Tidepool for technical documentation that doesn't belong in a specific app/library/service repo.

## Working on these docs

At Tidepool, we are starting to use [GitBook](https://www.gitbook.com/) for documentation across our client-side apps and here in this repository for "no man's land" docs without a proper home in another repo. We use [GitHub Pages](https://pages.github.com/) to publish the GitBook-generated docs on the web at various paths under the base URL of our developer microsite at [http://developer.tidepool.io/](http://developer.tidepool.io/). If you need to add or update the docs in this repo or in another repo that employs GitBook for publishing documentation, follow the instructions below.

### Setting up your repo to work on docs

When you use GitHub Pages to host a website for a particular project (like this one), you must create a branch titled `gh-pages` that contains all the files for your (static) website. Since for most repositories, this `gh-pages` branch contains content that is very different from other branches, [GitHub's advice](https://help.github.com/articles/creating-project-pages-manually/) is to use a separate clone of the repository to manage *just* the `gh-pages` branch. This is what jebeck does, and she recommends the following set up procedure:

1. Clone another copy of this repository, nested inside your top-level `docs` directory, but give it the name 'web': `git clone git@github.com:tidepool-org/docs.git web`. (The optional final argument to `git clone` tells git to name the cloned directory something other than the default repo name.)
1. Navigate into the new clone with `cd web/`.
1. Checkout the gh-pages branch in the new clone with `git checkout gh-pages`.
1. Delete the `master` branch in the new clone since you will *only* be using this clone for managing the GitHub Pages code for the repo: `git branch -d master`.
1. Test that you can view the GitHub Pages site locally by serving up the site to yourself (e.g., with the Python one-liner `python -m SimpleHTTPServer`) and viewing it in a browser.

(Example given for this repository `docs`, but the same procedure is recommended for all Tidepool repos using GitBook, just substitute the repo name (e.g., `chrome-uploader`) for `docs` in the above instructions.)

### Adding or updating docs

The [GitBook documentation](https://help.gitbook.com/) is a good, well-organized resource, but the basics are very simple and summarized here.

GitBook is a tool for compiling documentation from [Markdown](https://daringfireball.net/projects/markdown/) files. At Tidepool, thus far our only target for compilation is a static website that can be hosted on GitHub Pages, but GitBook offers other compilation targets, including PDF and e-reader formats.

At the top level, GitBook requires at minimum a `README.md`, which serves as the landing site for the documentation. (This `README.md` you are reading right now is thus the main page content for [http://developer.tidepool.io/docs/index.html](http://developer.tidepool.io/docs/index.html).) A `SUMMARY.md` is also required; the `SUMMARY.md` is where the docs are organized hierarchically, and one very nice thing about GitBook is that the hierarchy as described in the `SUMMARY.md` does *not* have to match the directory structure of the repository! If a Markdown file is *not* linked in the `SUMMARY.md`, it will **not** be compiled by GitBook (and may appear in your browser as plain Markdown), even if it is linked from another file that *is* linked in the `SUMMARY.md`. This issue of a missing link to a new file from the `SUMMARY.md` often surfaces with the error message "[some page] contains an hyperlink to resource outside spine [unlinked Markdown file]" during compilation. Basically, if you see that error, check the `SUMMARY.md` to make sure it includes everything it should.

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
