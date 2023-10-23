# Working draft specs

The version folder is used to keep work-in-progress draft specs.

## Linting and spell checking

API specifications are checked with `cspell` for spelling errors, and with Redocly CLI linter for correctness.  While never a 100% guarantee of correctness, these help find issues associated with changes to the OpenAPI specifications.

The [linter configuration](api-lint-config.yaml) contains rules that extend Redocly's `recommended` set of linting rules. To use this run the following command (with path to OpenAPI specification):

`npx @redocly/cli --config api-lint-config.yaml lint <path_to_openapi>`

The [spell check configuration](cspell.json) contains exclusion words that are specific to the API and can be used with the following command:

`npx cspell --config cspell.json <path_to_openapi>`
