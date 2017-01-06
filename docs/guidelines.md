## Guidelines for Tidepool developer documentation

Jump to...
- [Self-documenting code](#self-documenting-code)
- [External documentation](#external-documentation)
- [Suggested topics](#suggested-topics)

### Self-documenting code

All code should be self-documenting such that the code structure, class names, method names, function names, and variable names all help to clearly communicate the intention of the code.

All code should be well formatted, use expressive names, and consist of short, single-responsibility functions, methods, and classes.

Inline comments may be used for documenting code as necessary. Where that documented knowledge is useful outside of the context of the code, consider moving that knowledge to external documentation or at least providing a link from the external documentation to the inline comments.

### External documentation

External documentation should communicate big-picture concepts, architecture, design, algorithms, and larger implementation details.

External documentation should:
- Be written in GitHub-style Markdown.
- Be committed to the GitHub repository most closely associated with the topic.
- Be located "as close as possible" to the related code.
    + The preferred location is the `README.md` file in the parent directory of the code.
    + Additional required files (e.g. multiple large topics, images, supporting files) should be stored in the `docs` directory in the parent directory.
        - The `README.md` should reference any additional required files.
        - Images should be exported as PNG.
        - Original source files (e.g. Photoshop or SVG source for PNG) should also be stored in the `docs` directory.
- Consider using diagrams and graphics to communicate more clearly than words.
- Include a root `README.md` that references all other `README.md` files in the repository.
    + If the repository is one that uses GitBook for building a static website (hosted on GitHub Pages) of the documentation, the `SUMMARY.md` will contain the references to all other documentation, but the `README.md` should still link into the top-level documentation sections (since it gets built as the `index.html` of the GitBook-generated static site).
- Be [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself). If the information exists elsewhere, refer to that, do not repeat. For example, include links to any associated Trello cards rather than duplicating information.
- Consider the [docs](https://github.com/tidepool-org/docs 'Tidepool on GitHub: docs') repository as the starting point of all documentation that should:
    + Include a root `README.md` that references all other documentation in the repository and all documentation in other GitHub repositories.
    + Include documentation that does not naturally belong in another GitHub repository.

#### Suggested topics

Consider the following topics for each piece of external (i.e., not inline) documentation:
- Brief description
- Architectural and design overview
- User experience
    + Consider referring to related UX design files and documents.
- Module, component, and code organization
    + Consider using simple diagrams to communicate more effectively than words for such things as a React component hierarchy.
- External code dependencies, integrations, and relationships
- Known issues, edge cases, caveats, gotchas, hacks, tech debt, and bugs
    + Bugs should be fully documented in a Trello card and referenced here.
- Deprecation notes

Use your best judgement when documenting. Obviously, not all documentation will neatly fit into these topics. Add and remove topics as you deem appropriate.
