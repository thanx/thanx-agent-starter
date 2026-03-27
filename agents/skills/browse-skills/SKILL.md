---
name: browse-skills
description: Browse skills from all repos in the central skill ecosystem registry
---

# Browse Skills

Discover and browse skills from other people's repos without cloning them. Reads the central
`community/repo-list.yaml` from `thanx/thanx-agent-starter` and lets you explore what's available.

## When to Use

- You want to see what skills other people have built
- You're looking for a skill that solves a specific problem
- You want to adopt a skill from someone else's repo

## Process

### 1. Fetch the Repo List

```bash
gh api "repos/thanx/thanx-agent-starter/contents/community/repo-list.yaml" --jq '.content' | base64 -d
```

Parse the YAML to get the list of repos.

### 2. Present the Registry

Show all registered repos:

```markdown
## Skill Ecosystem — Registered Repos

| # | Name | Owner | Type | Description |
|---|------|-------|------|-------------|
| 1 | thanx-cortex | Thanx Engineering | hub | Org-wide engineering toolkit |
| 2 | kosmin-claude-skills | Kosmin | marketplace | Discovery, curation, distribution |
| 3 | anutron-claude-skills | Aaron | personal | Bug-bash, fixit, improve |
| 4 | drn-dots | Darren | claude-dir | Original Claude skills |
```

Ask: **"Which repo would you like to browse? (number, name, or 'all')"**

### 3. Fetch Skills from Selected Repo

For the chosen repo, fetch its skill directory tree:

```bash
# Extract owner/repo from the remote URL
# e.g., git@github.com:anutron/claude-skills.git -> anutron/claude-skills

gh api "repos/{owner}/{repo}/git/trees/main?recursive=1" --jq '.tree[] | select(.type == "blob") | .path'
```

Filter for skill files based on the repo's `skills_path`:
- Files matching `{skills_path}/**/SKILL.md`
- Or `{skills_path}/**/*.md` (for flat structures)
- Skip README.md, AGENTS.md, CLAUDE.md

### 4. Present Available Skills

For each skill file found, fetch its content and extract frontmatter:

```bash
gh api "repos/{owner}/{repo}/contents/{path}" --jq '.content' | base64 -d
```

Display:

```markdown
## Skills in {repo_name} ({count} found)

| # | Skill | Description |
|---|-------|-------------|
| 1 | improve | End-of-session learning loop |
| 2 | bug-bash | Systematic bug hunting workflow |
| 3 | fixit | Quick fix with automated testing |
```

Ask: **"Which skill would you like to read? (number, or 'adopt' to copy one locally)"**

### 5. Read a Skill

Show the full SKILL.md content for the selected skill.

### 6. Adopt a Skill (Optional)

If the user wants to adopt a skill:

1. Copy the SKILL.md content
2. Create the directory: `agents/skills/{skill-name}/`
3. Write the SKILL.md file
4. Report: "Adopted `{skill-name}` from `{repo_name}`. Available as `/{skill-name}` in your next session."

## Error Handling

- **Repo not accessible**: Check if it's private. Suggest `gh auth login` or ask the repo owner for access.
- **No skills found**: The repo may use a different structure. Show the file tree and let the user pick.
- **Rate limit**: Suggest browsing one repo at a time or cloning locally.
- **Repo-list not found**: The `community/repo-list.yaml` may not exist yet. Suggest checking back later.
