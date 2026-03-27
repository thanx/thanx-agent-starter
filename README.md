# thanx-agent-starter

A starter kit for building your personal AI development ecosystem. Fork it, fill in your context, and start compounding.

## What This Is

This repo gives you the self-improving infrastructure that makes AI-assisted development compound over time. It includes four core meta-skills and a structure for accumulating knowledge across sessions. The skills are not workflow-specific — they are the infrastructure that makes all your future skills get better.

The core skills and patterns originated from [Darren Cheng's](https://github.com/drn) personal AI workflow, demonstrated at the Thanx AI Academy (Feb 2026). This repo generalizes that system into a forkable starting point.

## Philosophy

- **Fork and diverge.** Clone this, make it yours. Your personal repo should reflect how YOU work.
- **The improve loop is everything.** Run `/improve` at the end of sessions. Each session leaves the system better than it found it.
- **Start personal, promote when ready.** Build skills for yourself first. When something proves universal, contribute it back to the core.
- **Steal freely.** Browse other people's forks for ideas. Copy what works. Adapt it. No permission needed.

## The Three-Layer Model

Before you fork, understand where skills live. This mental model prevents the most common mistake — dumping everything into your personal repo and wondering why project-specific skills don't transfer.

```
┌─────────────────────────────────────────────────┐
│  Layer 3: Team-wide (thanx-claude-plugins)      │
│  Standards everyone follows. Shared reviews,    │
│  investigation workflows, ops patterns.         │
├─────────────────────────────────────────────────┤
│  Layer 2: Project-specific (.claude/commands/)  │
│  Skills tied to a specific codebase. Deploy     │
│  scripts, test runners, project conventions.    │
├─────────────────────────────────────────────────┤
│  Layer 1: Personal (this repo)                  │
│  YOUR workflow, preferences, automation.        │
│  /improve, /handoff, voice profile, knowledge.  │
└─────────────────────────────────────────────────┘
```

**Start at layer 1.** Build skills for yourself. When something proves useful beyond your personal workflow, promote it to layer 2 (project) or layer 3 (team). This repo is your layer 1.

## Quick Start

```bash
# Fork and clone
gh repo fork thanx/thanx-agent-starter --clone
cd thanx-agent-starter

# Run setup (creates symlinks to ~/.claude/)
./setup.sh

# Fill in your context
$EDITOR global/CLAUDE.md      # Your identity, preferences, org context
```

Then start using Claude Code in any project. The skills are available everywhere via `/disk-cleanup`, `/improve`, `/handoff`, `/knowledge`, `/write-skill`, `/sync-upstream`, `/browse-skills`, and `/register-repo`.

## What Is Included

### Core Skills

| Skill | Purpose |
|-------|---------|
| `/disk-cleanup` | Scan local disk for large storage consumers and identify cleanup opportunities. Read-only by default, never deletes without approval. |
| `/improve` | End-of-session learning loop. Extracts friction points, proposes skill improvements, captures knowledge. The compounding mechanism. |
| `/handoff` | Generates structured context prompts for transferring work between agent sessions or repos. |
| `/knowledge` | Manages a structured knowledge base for facts, patterns, and insights that persist across sessions. |
| `/write-skill` | Skill authoring guide with platform conventions, gotchas, and validation. Makes your first custom skill work correctly. |
| `/sync-upstream` | Checks the upstream scaffold for improvements and helps you incorporate them into your fork. |
| `/browse-skills` | Browse skills from all repos in the central ecosystem registry. Discover and adopt without cloning. |
| `/register-repo` | Register this repo in the central skill ecosystem registry via PR. Makes your skills discoverable. |

### Templates

| File | Purpose |
|------|---------|
| `global/CLAUDE.md` | Your global instructions, loaded into every conversation. Fill in your context. |
| `context/voice-profile.md` | Instructions for generating a writing voice profile from your emails and messages. |
| `context/knowledge/index.md` | Empty knowledge base registry. Grows as you capture learnings. |

## Repository Structure

```
thanx-agent-starter/
├── README.md                          # This file
├── CLAUDE.md                          # Instructions for working IN this repo
├── setup.sh                           # Creates symlinks to ~/.claude/
├── global/
│   └── CLAUDE.md                      # Template -> symlinked to ~/.claude/CLAUDE.md
├── agents/
│   └── skills/                        # Core skills -> symlinked to ~/.claude/skills/
│       ├── disk-cleanup/SKILL.md
│       ├── improve/SKILL.md
│       ├── handoff/SKILL.md
│       ├── knowledge/SKILL.md
│       ├── write-skill/SKILL.md
│       ├── sync-upstream/SKILL.md
│       ├── browse-skills/SKILL.md    # Browse skills from ecosystem
│       └── register-repo/SKILL.md    # Register repo in central list
├── context/
│   ├── voice-profile.md              # Instructions for generating yours
│   └── knowledge/
│       └── index.md                   # Empty knowledge base registry
├── community/
│   ├── repo-list.yaml                # Central skill repo registry
│   └── README.md                     # Ecosystem documentation
└── .gitignore
```

## How It Works

### The Symlink Model

The setup script creates two symlinks:

- `~/.claude/CLAUDE.md` -> `<your-repo>/global/CLAUDE.md`
- `~/.claude/skills/` -> `<your-repo>/agents/skills/`

This means your global instructions and skills are version-controlled in your fork, but Claude Code picks them up automatically in every project.

### The Improve Loop

The core workflow:

1. Work in any project using Claude Code or Conductor
2. At the end of the session, run `/improve`
3. It analyzes the session, identifies friction, proposes skill improvements
4. Improvements get applied to your skills, knowledge gets captured
5. Next session starts with a better system

This is the compounding mechanism. Without it, skills are static markdown. With it, every session makes the system smarter.

## Staying Current with Core

Run `/sync-upstream` from within this repo to check for improvements to core skills. It will:

1. Fetch the latest from the upstream scaffold
2. Show you what changed with a recommendation for each (adopt, skip, or review)
3. Help you merge selected changes without overwriting your personal content

You can also do it manually:

```bash
# Add upstream remote (one-time)
git remote add upstream git@github.com:thanx/thanx-agent-starter.git

# See what changed
git fetch upstream
git log HEAD..upstream/master --oneline

# Cherry-pick specific improvements, or ask Claude to help incorporate them
```

## Generating Your Voice Profile

One of the most impactful early steps is generating a voice profile — a document that captures how you actually write so Claude can match your style.

See `context/voice-profile.md` for instructions. The short version: point Claude at your last 30 days of outgoing emails and Slack messages, and ask it to extract your writing patterns.

## Contributing Back

Found an improvement to a core skill that would benefit everyone? Open a PR against `thanx/thanx-agent-starter`. Good candidates for upstream contributions:

- Bug fixes in core skills
- New platform conventions discovered (Claude Code gotchas, dynamic context patterns)
- Structural improvements to the improve loop or knowledge capture
- Better template sections in the CLAUDE.md

NOT good candidates for upstream:
- Workflow-specific skills (expense reports, leadership prep, etc.)
- Org-specific context or cached IDs
- Personal preferences baked into core skills

## License

Internal to Thanx. Fork freely within the organization.

## Skill Ecosystem

This starter participates in a decentralized skill-sharing ecosystem. Everyone's personal repo
can be discovered by others — no central authority needed, just a shared list.

### Central Repo-List

`community/repo-list.yaml` lists all known skill repos. Anyone can add theirs via PR.

### Discovering Others' Skills

```bash
/browse-skills          # Browse skills from all registered repos (no cloning needed)
/register-repo          # Register your repo in the central list via PR
```

### Current Registry

| Repo | Owner | Type | Description |
|------|-------|------|-------------|
| [thanx-cortex](https://github.com/thanx/thanx-cortex) | Thanx Engineering | hub | Org-wide engineering toolkit |
| [kosmin-claude-skills](https://github.com/thanx/kosmin-claude-skills) | Kosmin | marketplace | Discovery, curation, distribution |
| [anutron-claude-skills](https://github.com/anutron/claude-skills) | Aaron | Personal | Bug-bash, fixit, improve |
| [drn-dots](https://github.com/drn/dots) | Darren | claude-dir | Original Claude skills |

### Centralization Flow

When 3+ people have similar skills, they're candidates for promotion to `thanx-cortex`:

1. Build skills in your personal repo
2. Others discover them via `/browse-skills`
3. When a skill proves universal, push it to `thanx-cortex` as a PR
4. Everyone benefits from the centralized version

See `community/README.md` for full details.
