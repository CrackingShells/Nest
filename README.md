# Nest

Nest is the `CrackingShells` org's plugin catalogue. It owns the `cracking-shells`
marketplace name across every client that resolves agent plugins from a git
source — Claude Code and Codex today. Product and playbook repositories no
longer ship a marketplace of their own; they are listed here by git source
instead, so the org has exactly one place a client can register the
`cracking-shells` name from.

## Install

### Claude Code

```bash
claude plugin marketplace add CrackingShells/Nest
claude plugin install <plugin>@cracking-shells
```

### Codex

```bash
codex plugin marketplace add CrackingShells/Nest
codex plugin add <plugin>@cracking-shells
```

Replace `<plugin>` with any of the plugin names listed below.

## Migrating from the colgrep-mcp marketplace

`colgrep-mcp` used to ship its own marketplace, also named `cracking-shells`,
directly inside `CrackingShells/colgrep-mcp`. A client keeps whichever
marketplace it registered under a given name at add time, so if you already
ran `claude plugin marketplace add CrackingShells/colgrep-mcp` (or the Codex
equivalent), your client is still pointed at that old, smaller catalogue —
**with no error shown**. Adding Nest under the same name does not
automatically replace it for you.

Before adding Nest, remove the old registration first:

```bash
claude plugin marketplace remove cracking-shells
# or, for Codex:
codex plugin marketplace remove cracking-shells
```

Then add Nest as described above. Skipping the removal step means you keep
seeing only `colgrep-mcp` and `colgrep-mcp-dev`, silently missing every
playbook plugin.

## Plugins

| Plugin | Repository | Description |
|:-------|:-----------|:------------|
| `managing-roadmaps` | cracking-shells-playbook | Creates, executes, and amends graph-based campaign roadmaps. |
| `writing-history` | cracking-shells-playbook | Guides the git-discipline lifecycle for commit history: vocabulary, versioning, commits, and branch integration. |
| `writing-release` | cracking-shells-playbook | Writes GitHub pull request bodies and Discord release announcements for fork-based release workflows. |
| `writing-reports` | cracking-shells-playbook | Guides production of stakeholder-reviewable reports for coding, research, and writing work. |
| `spawning-agent-plugins` | cracking-shells-playbook | Turns a repository into an installable plugin for Claude Code, Codex, and Agent Plugins 1.0 clients. |
| `waiting-on-processes` | Pinion | Writes waits and watches in Claude Code that end by themselves and catch failures, with a sweep that stops stray processes. |
| `writing-prose` | Plumage | Drafts, revises and reviews prose signed in the author's own voice, with register control against LLM-default rhetoric. |
| `writing-design-canons` | Plumage | Produces layered design canons a team uses to make assets in a coherent style and stakeholders use to review them. |
| `importing-design-figures` | Plumage | Turns a Claude Design figure or table into a print-quality image placed, undistorted, into a local document. |
| `coining-neologisms` | Plumage | Coins a precise new word and its morphological family for a concept that lacks a compact name. |
| `pdf-to-md` | Plumage | Converts digital-native PDFs into Markdown with figures embedded where they belong, using poppler and pandoc. |
| `colgrep-mcp` | colgrep-mcp | Semantic and hybrid code search for coding agents, exposed as MCP tools backed by colgrep. |
| `colgrep-mcp-dev` | colgrep-mcp | **Maintainer-only.** Maintainer skills for the colgrep-mcp repository (policy, campaign leadership, landing and release, stack traps). Not for end users. |

No entry here carries a `version`, `ref` or `sha`. Claude Code resolves each
plugin's own `plugin.json` version and falls back to the source commit SHA
when none is set, so this catalogue never needs a release-time edit — every
plugin updates whenever its own source moves.

## Adding a plugin

Nest only lists plugins; it never holds their content. To list a new one:

1. **Make the source repository a hub-mode plugin repo.** Its `plugin.spec.json`
   sets `"marketplace": {"hub": "https://github.com/CrackingShells/Nest"}`, so
   `spawning-agent-plugins` writes the plugin manifests but no marketplace of
   its own. Two marketplaces under the `cracking-shells` name collide silently
   in Claude Code and fail in Codex.
2. **Push the plugin root first.** Each entry installs from the source
   repository's default branch, so the plugin directory must exist there before
   Nest points at it.
3. **Add one entry to each catalogue**, in the same order in both:
   - `.claude-plugin/marketplace.json`: `name`, `source`, `displayName`,
     `description`, `category`;
   - `.agents/plugins/marketplace.json`: the same fields, plus
     `"policy": {"installation": "AVAILABLE", "authentication": "ON_INSTALL"}`.

   The `source` is `{"source": "git-subdir", "url": "https://github.com/CrackingShells/<repo>.git", "path": "<dir>"}`
   for a plugin in a subdirectory (`<dir>` is the `dir` of its spec entry), or
   `{"source": "url", "url": "https://github.com/CrackingShells/<repo>.git"}`
   for a plugin rooted at the repository. Copy `displayName`, `description` and
   the Codex `category` from the spec entry, so the catalogue and the plugin's
   own manifests say the same thing. Add no `version`, `ref` or `sha` (see
   above).
4. **Add a row to the table above**, and mark a maintainer-only plugin as such.
5. **Check:** `claude plugin validate .` must pass, then
   `claude plugin marketplace update cracking-shells` and
   `claude plugin install <plugin>@cracking-shells` from a client that already
   has Nest registered.

Moving a plugin to another repository uses the same steps: change its
`source.url` (and `path`, if it moved) in both catalogues and its row in the
table, after the new repository's plugin root is pushed.
