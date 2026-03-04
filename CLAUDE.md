# thanx-agent-starter

Personal AI workflow repo. Contains global Claude Code configuration, personal skills, knowledge base, and voice profile.

## Repository Structure

```
thanx-agent-starter/
├── CLAUDE.md                          # This file (repo-level instructions)
├── global/
│   └── CLAUDE.md                      # Global instructions -> symlinked to ~/.claude/CLAUDE.md
├── agents/
│   └── skills/                        # Personal skills -> symlinked to ~/.claude/skills/
│       ├── disk-cleanup/SKILL.md
│       ├── improve/SKILL.md
│       ├── handoff/SKILL.md
│       ├── knowledge/SKILL.md
│       └── write-skill/SKILL.md
├── context/
│   ├── voice-profile.md              # Generated voice/writing profile
│   └── knowledge/                    # Personal knowledge base
│       └── index.md
└── .gitignore
```

## Symlink Setup

Run `./setup.sh` or manually create:

- `~/.claude/CLAUDE.md` symlinked to `global/CLAUDE.md`
- `~/.claude/skills/` symlinked to `agents/skills/`

## Working With Skills

Skills follow the Claude Code SKILL.md format. See `/write-skill` for the authoring guide.

### Dynamic Context Rules (Critical)

- No `$()` in dynamic context (blocked by Claude Code security)
- No `||` or `&&` in dynamic context (permission system blocks these)
- Always use `2>/dev/null | head -N` to suppress errors and neutralize exit codes
- Use `origin/HEAD` instead of hardcoding branch names
- Keep output bounded with `| head -N`

### Testing Skills

After editing a skill, test it by running the `/skill-name` command in any project. Skills are loaded from the symlinked directory so changes are immediately available.

## Conventions

- Keep skills concise and imperative
- Use numbered steps for predictable execution
- Include abort conditions and loop limits
- No backticks in prose (shell evaluation breaks them)
- No contractions (single-quote interpretation issues)
