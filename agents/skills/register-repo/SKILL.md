---
name: register-repo
description: Register this skill repo in the central ecosystem registry via PR
---

# Register Repo

Registers the current repo in the central skill ecosystem registry (`community/repo-list.yaml`
in `thanx/thanx-agent-starter`) by creating a pull request.

## When to Use

- You've forked thanx-agent-starter and added your own skills
- You've created a standalone skill repo
- You want others to discover your skills via `/browse-skills`

## Process

### 1. Gather Repo Info

```bash
# Get the remote URL
REMOTE=$(git remote get-url origin 2>/dev/null)

# Count skills
SKILL_COUNT=$(find agents/skills -name "SKILL.md" 2>/dev/null | wc -l)
# Fallback: check other common paths
if [ "$SKILL_COUNT" -eq 0 ]; then
  SKILL_COUNT=$(find skills -name "SKILL.md" 2>/dev/null | wc -l)
fi
if [ "$SKILL_COUNT" -eq 0 ]; then
  SKILL_COUNT=$(find .claude/skills -name "*.md" 2>/dev/null | wc -l)
fi

# Detect skills path
if [ -d "agents/skills" ]; then
  SKILLS_PATH="agents/skills/"
elif [ -d "skills" ]; then
  SKILLS_PATH="skills/"
elif [ -d ".claude/skills" ]; then
  SKILLS_PATH=".claude/skills/"
fi
```

### 2. Check if Already Registered

```bash
EXISTING=$(gh api "repos/thanx/thanx-agent-starter/contents/community/repo-list.yaml" --jq '.content' | base64 -d)
```

Search for the remote URL in the existing repo-list. If found, report:

> "This repo is already registered in the central repo-list."

And stop.

### 3. Determine Repo Type

Ask the user or auto-detect:

- **marketplace**: Has `.claude-plugin/marketplace.json` or provenance tracking
- **personal**: Has `agents/skills/` with SKILL.md files (typical fork of agent-starter)
- **claude-dir**: Has `.claude/skills/` but no dedicated skill structure

### 4. Build the Entry

Ask the user to confirm:

```markdown
## Register in Central Repo-List

| Field | Value |
|-------|-------|
| Name | {suggested from repo name} |
| Owner | {from git config or ask} |
| Remote | {REMOTE} |
| Type | {detected type} |
| Description | {ask user for a brief description} |
| Skills Path | {SKILLS_PATH} |
| Skills Count | {SKILL_COUNT} |
```

**"Does this look correct? (yes/edit)"**

### 5. Create the PR

```bash
# Fork thanx-agent-starter if not already forked
gh repo fork thanx/thanx-agent-starter --clone=/tmp/agent-starter-fork --remote 2>/dev/null || \
  gh repo clone thanx/thanx-agent-starter /tmp/agent-starter-fork

cd /tmp/agent-starter-fork
git checkout -b add-repo/{repo-name}

# Append entry to community/repo-list.yaml
cat >> community/repo-list.yaml << EOF

  - name: "{name}"
    owner: "{owner}"
    remote: "{remote}"
    type: {type}
    description: "{description}"
    skills_path: "{skills_path}"
    registered_at: "{today}"
EOF

git add community/repo-list.yaml
git commit -m "Add {name} to skill repo registry"
git push -u origin add-repo/{repo-name}

gh pr create --repo thanx/thanx-agent-starter \
  --title "Add {name} to skill repo registry" \
  --body "Registers {name} ({SKILL_COUNT} skills) in the central repo-list for ecosystem discovery.

## Repo Details
- **Owner:** {owner}
- **Remote:** {remote}
- **Type:** {type}
- **Skills:** {SKILL_COUNT} skills at \`{skills_path}\`
- **Description:** {description}"
```

### 6. Report

```markdown
## Registered!

**PR:** {pr_url}

Once merged, others can discover your skills via:
- `/browse-skills` (from thanx-agent-starter)
- `/snoop --fetch-central` (from kosmin-claude-skills marketplace)

### Next Steps
- Share the PR link with your team
- Ask a reviewer to merge it
```

## Error Handling

- **No remote configured**: Ask user for the repo URL
- **Fork permission denied**: Suggest creating the PR manually
- **`gh` CLI not available**: Provide manual instructions
- **community/repo-list.yaml doesn't exist**: Suggest the file may not have been created yet
