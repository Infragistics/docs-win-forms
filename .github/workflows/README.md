# Japanese Docs Sync Workflow

This folder contains an agent workflow used to keep Japanese AsciiDoc topics in sync with English topics.

## Files

- `sync-jp-docs.md` - Copilot agent workflow prompt for syncing `topics/en` updates into `topics/ja`.

## What This Is

`sync-jp-docs.md` is a **Copilot agent workflow definition** (frontmatter + instructions), not a traditional GitHub Actions YAML file.

The agent:

1. Detects English `.adoc` files changed in the latest push.
2. Finds the matching Japanese file path.
3. Applies equivalent content changes with Japanese translation for prose.
4. Opens a PR with synced updates.

## Triggers

Configured in the workflow frontmatter:

- `push` on branches:
  - `main`
  - `gedinakova/jp-translation-aw`
- path filter:
  - `topics/en/**/*.adoc`
- manual trigger:
  - `workflow_dispatch`

## Path Mapping Rules

For each changed English file:

- Source: `topics/en/<name>.adoc`
- Target resolution order:
  1. `topics/ja/<name>.ja-JP.adoc`
  2. `topics/ja/<name>.adoc` (legacy fallback if it already exists)
- If neither exists, create:
  - `topics/ja/<name>.ja-JP.adoc`

## Branch and PR Behavior

- Safe output `create-pull-request` targets base branch: `main`
- PR title prefix: `[jp-sync] `
- Labels: `translation`, `japanese`, `automation`
- If there are no applicable changes, the workflow emits a no-op output.

## Translation Rules (Summary)

Translate:

- Titles and headings
- Paragraphs and list text
- Table prose

Preserve exactly:

- AsciiDoc metadata block structure
- Tokens such as `{ProductName}`, `{PlatformName}`, `{ApiVersion}`
- AsciiDoc directives (`ifdef::`, `endif::`, `include::`, etc.)
- Code blocks and source fences
- Link/image targets, URLs, API names, CLI commands

## How To Run

### Automatic

1. Commit and push changes under `topics/en/**/*.adoc` to a configured branch.
2. The workflow runs automatically.

### Manual

1. Open repository Actions in GitHub.
2. Select **Sync Japanese Documentation**.
3. Click **Run workflow** and choose branch.

## Notes

- No compilation/build step is required for this workflow.
- This is content synchronization and translation automation.
- To add document validation (lint/link checks), create a separate CI workflow.