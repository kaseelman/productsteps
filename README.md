# PM Knowledge OS — Productsteps

A git-managed, LLM-queryable knowledge base for the product team building a comprehensive travel app across 7 product surfaces: Inspire, Track, Plan, Relive Physical, Relive Digital, Users, and Focus Projects.

## Folder structure

| Folder | What lives here |
|---|---|
| `context/` | Cross-domain knowledge: strategy, decisions, roadmap, research, stakeholders |
| `domains/` | One folder per product surface; each has context, features, decisions, research |
| `inbox/` | Raw, unprocessed drops — transcripts, Slack exports, links |
| `docs/` | Contribution guide and reusable templates |
| `.claude/` | Skills and commands for Claude Code workflows |

## How to contribute

See [docs/contribution-guide.md](docs/contribution-guide.md) for the full model — folder ownership, naming conventions, frontmatter requirements, and rules for keeping the repo healthy.

## Using this repo with Claude Code

Open this repo in Claude Code and it will automatically load `CLAUDE.md` as its briefing. Ask domain-specific questions after pointing Claude to the right `domains/<domain>/context.md`, or ask cross-domain questions and Claude will load `context/strategy/` first. Use `/inbox-triage` to process raw content from `inbox/`.
