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

| Plugin | Description |
|:-------|:------------|
| `managing-roadmaps` | Creates, executes, and amends graph-based campaign roadmaps. |
| `writing-history` | Guides the git-discipline lifecycle for commit history: vocabulary, versioning, commits, and branch integration. |
| `writing-release` | Writes GitHub pull request bodies and Discord release announcements for fork-based release workflows. |
| `writing-reports` | Guides production of stakeholder-reviewable reports for coding, research, and writing work. |
| `spawning-agent-plugins` | Turns a repository into an installable plugin for Claude Code, Codex, and Agent Plugins 1.0 clients. |
| `colgrep-mcp` | Semantic and hybrid code search for coding agents, exposed as MCP tools backed by colgrep. |
| `colgrep-mcp-dev` | **Maintainer-only.** Maintainer skills for the colgrep-mcp repository (policy, campaign leadership, landing and release, stack traps). Not for end users. |

No entry here carries a `version`, `ref` or `sha`. Claude Code resolves each
plugin's own `plugin.json` version and falls back to the source commit SHA
when none is set, so this catalogue never needs a release-time edit — every
plugin updates whenever its own source moves.
