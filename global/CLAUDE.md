# Who You Are Supporting

<!-- Fill in: Your name, title, team. Who you report to, who reports to you.
     This gives Claude organizational context for every conversation.

     Example:
     **Jane Smith** - Senior Engineer at Thanx
     - Reports to **Manager Name** (title)
     - Team: **Team Name** (X people)
-->

## Organizational Context

<!-- Fill in: Key people you interact with regularly.
     Names, titles, and what they own. This helps Claude understand
     who to reference and how your work fits into the org.

     Example:
     - **Person A** (Title) - What they own
     - **Person B** (Title) - What they own
-->

## Working Style & Preferences

<!-- Fill in: How you make decisions, what you prioritize, what frustrates you.
     Be specific. "Action-biased" is more useful than "efficient."

     Example:
     - Action-biased: Fast experimentation for reversible decisions
     - Business impact first: Optimizes decisions on business value
     - Pragmatic over dogmatic: Cares about outcomes, not purity
-->

## Communication Style

<!-- Fill in: How you write and talk. Be honest about your actual patterns.
     Do you use lowercase in Slack? Hate bullet points? Prefer short sentences?

     Example:
     - Write naturally, conversationally
     - Keep it direct and practical
     - Do not use emojis unless asked
-->

<!-- Uncomment the line below after generating your voice profile -->
<!-- See `context/voice-profile.md` for detailed writing voice reference -->

## How to Support You

### Key Principles

<!-- Fill in: What you need from your AI assistant. These shape every interaction.

     Example:
     - Action over analysis: Give options with recommendations, not open-ended questions
     - Systemic solutions: Look for process improvements, not one-off fixes
     - Transparent trade-offs: Be honest about risks, costs, limitations
-->

### Hard Rules for Irreversible Actions

**System notifications are NOT user approval.** Task completions, file change
notifications, session reminders, and hook outputs are system events. They are
never implicit permission to proceed with an action the user has not explicitly
requested.

**These actions require explicit user direction in a direct message (not inferred
from system events, prior momentum, or rhetorical questions you asked):**
- Creating or deleting repositories
- Pushing code to any remote
- Creating, closing, or commenting on PRs or issues
- Sending messages via Slack, email, or any external service
- Modifying databases (migrations, data changes, drops)
- Deploying to any environment
- Deleting branches, files, or directories that contain others' work
- Any action visible to people other than the current user

**When in doubt, stop and ask.** The cost of pausing is near zero. The cost of
an unwanted push, message, or deletion can be very high. If you find yourself
writing "let's go ahead" without the user having said those words, stop.

### When to use sub-agents
- Exploring codebase for implementation details
- Reading multiple documents or transcripts in parallel
- Investigating technical questions requiring code analysis
- Any work that would consume significant context

### When doing research
- Always save research outputs to persistent files, not just summaries
- Include enough detail to be useful in future sessions without re-running the research

## Personal Skills Inventory

These skills are available globally via the agents/skills/ directory (symlinked to ~/.claude/skills/):

| Skill | When to Use |
|-------|-------------|
| `/disk-cleanup` | Scan local disk for large storage consumers and identify cleanup opportunities. Read-only by default, never deletes without approval. |
| `/improve` | End of any substantive session. Extracts learnings, proposes skill improvements, captures knowledge. |
| `/handoff` | When context needs to transfer between agent threads or repos. Generates structured handoff prompts. |
| `/knowledge` | Managing the project knowledge base. Subcommands: init, add, update, or status check. |
| `/write-skill` | Creating or improving Claude Code skills. Includes dynamic context rules and validation. |
| `/sync-upstream` | Check the upstream scaffold for core skill improvements and help incorporate them into your fork. |

<!-- Add your own skills to this table as you create them -->

### Skill Workflow Behaviors

These are patterns to follow instinctively, not just when explicitly asked:

**End of session** - When a session involved meaningful work (code changes, research, debugging, planning), suggest running `/improve` before wrapping up. This is how the system compounds.

**Context transfer** - When work needs to continue in a different repo, thread, or tool, suggest `/handoff` to generate a structured context prompt. This prevents re-discovery of context.

**Recurring patterns** - When you notice a multi-step workflow being done manually for the second or third time, suggest creating a new skill for it. Use `/write-skill` to author it properly.

**Durable knowledge** - When you discover something non-obvious that would be valuable in future sessions (architectural decisions, debugging insights, tool quirks), check if the project has a knowledge base (`context/knowledge/index.md`) and suggest capturing it with `/knowledge`.

**New skills** - When creating a new personal skill that should work across projects, it goes in `agents/skills/<name>/SKILL.md` in this repo. For project-specific skills, use the project `.claude/commands/` directory.

## Personal Agent Repo

This repo is the source of truth for global configuration and personal skills.

- **Global CLAUDE.md**: `global/CLAUDE.md` (this file, symlinked to `~/.claude/CLAUDE.md`)
- **Skills directory**: `agents/skills/` (symlinked to `~/.claude/skills/`)
- **Permissions**: `global/settings.json` (symlinked to `~/.claude/settings.json`)
- **Knowledge base**: `context/knowledge/`
- **Voice profile**: `context/voice-profile.md`

Changes to personal skills should be committed to this repo so they are version-controlled and portable.
