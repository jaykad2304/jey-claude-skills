# Claude Skills Portfolio

A collection of custom Claude skills I've built — reusable AI workflows that extend Claude's behavior for specific tasks.

## What Are Claude Skills?

Skills are structured prompt files (`SKILL.md`) that teach Claude how to approach a specific task, pattern, or domain. They auto-trigger based on context and give Claude a proven playbook to follow — consistently, across any conversation.

Think of them as **AI workflow templates** you design once and reuse forever.

## Skills

| Skill | Description | Trigger |
|---|---|---|
| [idea-brainstorm](./idea-brainstorm/) | Full product strategy analysis across 8 dimensions | "I have an idea...", "Help me brainstorm..." |

## How to Use Any Skill

1. Copy the skill folder to your Claude skills directory:
   ```bash
   cp -r idea-brainstorm ~/.claude/skills/
   ```
2. Start a new Claude Code session
3. The skill loads automatically and triggers in context

## About

These skills are built using the [Claude Code](https://claude.ai/code) skill system. Each skill is designed to be:
- **Reusable** — works across projects and sessions
- **Structured** — consistent, predictable output
- **Community-shareable** — drop-in install for anyone using Claude Code

---

*Built by Jaykumar*
