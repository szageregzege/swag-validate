# HTML previews

This directory is reserved for generated HTML previews that reflect
the current state of the OpenAPI source files in the directory above.
Their format somewhat resembles the final publication on the
`techdocs` site, but does not exactly match.  Make sure to add these
additional non-source files to the branch.  Do *not* modify them.

To generate HTMLs, run `swag-tool preview` as summarized here:

- <https://techdocs.akamai.com/internal-ux-writing/docs/swag-preview>

See this page to install the tool:

- <https://techdocs.akamai.com/internal-ux-writing/docs/swag-get-started>

We recommend you run the `swag-tool preview` command before each
commit.  Doing so not only allows you to to view changes as you go,
but identifies many YAML/OpenAPI parse errors you might introduce.

## Merge conflicts

> __NOTE__: These HTML files are prone to merge conflicts, but there's
no need to resolve them. Simply merge in the latest `master`, then
overwrite them with a fresh set of HTMLs:

        $ git fetch origin master
        $ git merge FETCH_HEAD
        $ swag-tool preview
