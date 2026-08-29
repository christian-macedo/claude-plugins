# AGENTS.md

Notes for a coding agent working in this repository.

## What this repository is

A **Claude Code plugin marketplace index**, and nothing else. Four files. It contains no plugin
code and must never contain any.

```
.claude-plugin/marketplace.json   the index. the only file that carries meaning
README.md  AGENTS.md  LICENSE  .gitignore
```

Each plugin listed here lives in its own repository and is referenced by a pinned tag. If you are
asked to "add a plugin", that means **one entry in the `plugins` array**, not a directory of code.

## Do not change the marketplace name

```json
{ "name": "tousledmonkey" }
```

A user can register only one marketplace per name, and changing this field does **not** migrate
anyone. Existing users would have to remove the old marketplace and add the new one by hand. Treat
it as permanent.

Plugin names *are* migratable, with a `renames` map:

```json
{ "renames": { "old-plugin-name": "new-plugin-name" } }
```

## Adding a plugin

```json
{
  "name": "<plugin name, matching its plugin.json>",
  "source": { "source": "github", "repo": "<owner>/<repo>", "ref": "<plugin>--v<version>" },
  "description": "<one sentence, present tense>",
  "version": "<matching the tag and the plugin.json>"
}
```

Three values must agree, or a release is broken: the `version` here, the `version` in the plugin's
`plugin.json`, and the git tag named in `ref`. `claude plugin tag` in the plugin's repository
checks exactly that agreement and refuses when it fails.

Pin `ref` to a tag. Never point at `main`, or people install half-finished commits.

Then add a row to the table in [README.md](README.md).

## Verify

```bash
claude plugin validate .
```

Must pass. This is a JSON manifest, so a syntax error is the whole failure mode; there is no test
suite to run.

## Releasing a plugin listed here

Two steps, in this order. In the plugin's own repository, with `plugin.json` already bumped:

```bash
claude plugin tag --push
```

Then bump `ref` and `version` in `marketplace.json` here. Users pick it up with
`claude plugin update <name>`.

Doing it the other way round publishes an index entry pointing at a tag that does not exist yet.
