# Skill Ecosystem

Decentralized skill sharing across the Thanx org.

## How It Works

```
┌──────────────────────────────────────────────────────┐
│  Hub: thanx-cortex                                   │
│  Org-wide skills. Centralized when 3+ people share.  │
├──────────────────────────────────────────────────────┤
│  Central Registry: community/repo-list.yaml          │
│  Discovery aid. Lists all known skill repos.         │
├──────────────────────────────────────────────────────┤
│  Personal Repos: your-name-claude-skills             │
│  YOUR skills, YOUR workflow. Fork this starter.      │
└──────────────────────────────────────────────────────┘
```

1. **Personal repos** hold your skills (forks of this starter, or standalone)
2. **This file** (`community/repo-list.yaml`) lists all known repos for discovery
3. **thanx-cortex** is the hub — when a skill appears in 3+ personal repos, it's a candidate for promotion

## Adding Your Repo

Open a PR to add your repo to `community/repo-list.yaml`:

```yaml
  - name: "your-name-claude-skills"
    owner: "Your Name"
    remote: "git@github.com:your-org/your-repo.git"
    type: personal          # personal | marketplace | claude-dir
    description: "Brief description of what your skills focus on"
    skills_path: "agents/skills/"   # or "skills/", ".claude/skills/"
    registered_at: "YYYY-MM-DD"
```

## Discovering Skills from Others

### Using `/browse-skills` (from this starter)

Run `/browse-skills` to fetch the repo-list and browse skills from all registered repos.
No need to clone anything — it reads directly via the GitHub API.

### Using `/snoop --fetch-central` (from kosmin-claude-skills marketplace)

If you use the [kosmin-claude-skills](https://github.com/thanx/kosmin-claude-skills) marketplace:

```bash
/snoop --fetch-central   # Discover new repos from this list
/snoop all               # Scan all sources (shows centralization candidates)
```

## When to Centralize

A skill is a good candidate for `thanx-cortex` when:

- 3+ people have similar versions of it
- It's not personal workflow (it applies to anyone on the team)
- It's stable and proven (not a work-in-progress)

Use `/push-skill <name> thanx-cortex` to create a PR promoting a skill to the hub.

## Repo Types

| Type | Example | Description |
|------|---------|-------------|
| `hub` | thanx-cortex | Centralized, org-wide skills |
| `marketplace` | kosmin-claude-skills | Personal repo with discovery/push/sync tooling |
| `personal` | anutron-claude-skills | Personal repo, flat skills directory |
| `claude-dir` | drn/dots | Skills embedded in a dotfiles or project repo |
