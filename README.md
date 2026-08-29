# tousledmonkey

The Claude Code plugin marketplace for plugins I build. Adding it once makes every plugin
listed here installable, including ones added later.

## Use it

```bash
claude plugin marketplace add christian-macedo/claude-plugins
```

Then install whatever you want from it:

```bash
claude plugin install job-candidacy@tousledmonkey
```

Restart Claude Code to load it. `/plugin` does the same thing through a menu if you prefer one.

## What is here

| Plugin | What it does |
|---|---|
| [job-candidacy](https://github.com/christian-macedo/job-candidacy) | Tailored resumes, cover letters and interview prep from one canonical career record, where every claim cites the evidence behind it and a checker enforces it. |

## How this is put together

This repository holds **only** the index. Each plugin lives in its own repository and is
referenced by a pinned tag:

```json
{
  "name": "job-candidacy",
  "source": {
    "source": "github",
    "repo": "christian-macedo/job-candidacy",
    "ref": "job-candidacy--v1.0.0"
  }
}
```

Two things follow from that, both deliberate:

- **Each plugin keeps its own issues, CI and release cadence.** Someone contributing to one
  clones one repository and builds one thing.
- **Installs are pinned to a tag, not to `main`.** A plugin's default branch can move without
  anyone installing a half-finished commit. A release is a two-step: tag the plugin repository,
  then bump `ref` and `version` here.

## Releasing a new version

In the plugin's own repository, with `plugin.json` already bumped:

```bash
claude plugin tag --push
```

That creates `<name>--v<version>` and refuses if `plugin.json` and this marketplace's entry
disagree about the version. Then update `ref` and `version` in
[marketplace.json](.claude-plugin/marketplace.json) here, and users pick it up with
`claude plugin update <name>`.

## Adding a plugin

A new entry in the `plugins` array, pointing at its repository and tag. Nothing else moves.

## License

The index is MIT. Each plugin carries its own license, in its own repository.
