# CLAUDE.md — claude-meta

The **AI-collaboration layer** for the `claude-*` family: a coordination repo that sits above several independent Claude Code extension repos. It exists so that an agent working here gets the *whole* picture across those repos at once, governed by shared norms — instead of editing each repo blind to the others.

It holds coordination docs, not shippable code. Like the rest of the family it lives on a public GitHub remote, and `main` is protected — changes reach it through PRs.

## The family

Four content repos, each mapping 1:1 to one Claude Code extension mechanism, plus this coordination layer. All are siblings under `~/Developer/`:

| Repo | Extension type | Path |
|---|---|---|
| `claude-skills` | LLM-triggered skills | `../claude-skills` |
| `claude-hooks` | event-triggered hooks | `../claude-hooks` |
| `claude-agents` | subagent definitions | `../claude-agents` |
| `claude-mcps` | MCP servers | `../claude-mcps` |
| `claude-meta` | the coordination layer (this repo) | `.` |

Each content repo documents itself in its own `README.md`; read it before working there. Don't enumerate a repo's skills/hooks here — that list rots, and the repo's README is the real catalog.

## Why this layer exists

- **Coordinate without a monorepo.** The content repos are coupled (they change together) but must stay independently linkable, publishable, and ownable. A monorepo would bind them architecturally; a meta layer coordinates them without that cost.
- **Give the AI the full picture.** Cross-repo changes driven from inside one repo drift, because the agent can't see the others. Running coordination from here — with this file and the sibling repos in view — is the anti-drift mechanism, by context and norms rather than check scripts.
- **A reusable pattern.** This same "meta layer over N independent repos" shape is meant to extend beyond Claude tooling (frontend / backend / app clusters).

## How the family is wired

Each norm below is stated once; the authoritative definition lives in the skill or README pointed to — read it there rather than trusting a copy.

- **Pointer, not container.** This layer points at the content repos; it never holds their files. Each repo version-controls its real files and symlinks them into `~/.claude/` (`scripts/link-*.sh`); the runtime dir is never version-controlled. The siblings are **not** mirrored in-tree here — reach them at the relative paths `../claude-*` (real source at `~/Developer/claude-*`). A committed `.vscode/claude-meta.code-workspace` lists all five as VS Code roots for human review.
- **Naming.** `ultra-<single-token>-<verber>`; `author` is reserved for extension-*authoring* tools (`ultra-skill-author`, `ultra-hook-author`). The `ultra-` prefix is a personal-identity mark. Authoritative: `../claude-skills/skills/ultra-skill-author/SKILL.md`.
- **Provenance & licensing.** Per-item `LICENSE`; a `NOTICE` for derivatives; copyleft or unclear upstream → don't publish. Authoritative: `../claude-skills/skills/ultra-skill-author/references/publishing.md`.
- **Git ceremony.** One Conventional Commits vocabulary shared across branches, commits, and PRs — defined by `ultra-branch-creator` / `ultra-commit-creator` / `ultra-pr-creator` in `claude-skills`. Since `main` is protected, changes land as additive PRs — never by rewriting pushed history.
- **Self-hosting.** The family is stamped out and maintained by its own skills (`ultra-repo-creator` + `ultra-project-initializer` bootstrapped `claude-agents` / `claude-mcps`). The tools produce the family's own habitat.

## Working across the family

- **The coupling rule.** A change in one content repo often forces a matching change in `claude-skills` — e.g. renaming a hook/agent/mcp must also update the matching `*-author` skill's docs and examples (the `ultra-task-notifier` rename rippled into `ultra-hook-author`). When you touch a content repo, check whether its authoring skill references it.
- **Run coordination from here.** This file loads only in `claude-meta` sessions, so cross-repo work should be driven from this layer — reach the siblings at `../claude-*` (the workspace lists them too), so the whole family is in view. Scope `Grep`/`Glob` to those sibling paths explicitly; a bare `.` search stays inside this repo and won't reach them. Commits still land in each real repo (`~/Developer/claude-*`), never here.

## North star & judgment frame

- **Personal-fit is the hard constraint.** These repos serve four goals at once — personal tooling, a shareable toolkit, teaching artifacts, and capability exploration — but when fitting the maintainer's own workflow conflicts with outside usability, **personal-fit wins**. Hard-coded preferences (e.g. a fixed GitHub handle, kept local branch labels) and Chinese READMEs are deliberate; sharing is a door kept open, never a reason to generalize personal fit away.
- **Concrete and closed today, architecturally open tomorrow.** The recurring design signature: ship one concrete thing, leave the architecture open. It shows up in platform support (GitHub Pages now, others planned), the family boundary (four extension types now, room for more), and the eval machinery (active for objective-output skills, latent otherwise).
- **The family boundary.** Four content repos now; the architecture doesn't preclude later extension types (slash commands, plugins, …) but doesn't commit to them either.
