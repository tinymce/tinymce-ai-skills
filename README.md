# TinyMCE AI skills

A collection of **[Agent Skills](https://agentskills.io/)** for TinyMCE products, portable across AI coding agents (Claude Code, Codex, Cursor, GitHub Copilot, Google Antigravity, Windsurf, and any tool that supports the open skill format).

These skills give your coding agent deep, task-specific knowledge about TinyMCE so it can guide you through real work — no re-explaining your setup in every session.

## Skills

| Skill | Description |
| ----- | ----------- |
| [`tinymce-setup`](skills/tinymce-setup/SKILL.md) | Takes you from "I want a rich-text editor" to a working, correctly configured TinyMCE — environment detection, industry-based plugin recommendations, API key validation, and framework-specific integration code (React, Vue, Angular, Next.js, vanilla JS, and more). Also handles adding TinyMCE alongside an existing editor (CKEditor, Quill, Tiptap, Froala, and others). |

## Install

Install any skill in this repo with a single command using the [open skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add tinymce/tinymce-ai-skills
```

The CLI downloads the repo, detects which agent(s) you have installed, and copies the skill into the right place automatically. Once installed, the skill loads on its own whenever you ask your agent to install, set up, configure, or troubleshoot TinyMCE.

To install a specific skill from the repo:

```bash
npx skills add tinymce/tinymce-ai-skills --skill tinymce-setup
```

<details>
<summary><b>Manual installation (per agent)</b></summary>

<br>

If you'd rather not use the CLI, copy the skill folder into your agent's skills directory:

**Claude Code**
1. Copy the skill folder into `~/.claude/skills/` (global) or `.claude/skills/` in your project root (project-scoped).
2. Claude Code discovers the skill automatically and applies it when relevant.

**Codex**
1. Copy the skill folder into your project.
2. Register it as a Codex Skill via `AGENTS.md` so the agent can discover and apply it automatically.

**Cursor**
1. Copy the skill folder into your project.
2. Reference `SKILL.md` from your `.cursor/rules` file.

**GitHub Copilot**
1. Copy the skill folder into `.claude/skills/` in your project root.
2. Copilot discovers and loads it automatically when relevant — no extra configuration needed.

**Google Antigravity**
1. Copy the skill folder into your project.
2. Reference `SKILL.md` from your `GEMINI.md` file, or from `AGENTS.md` for a cross-tool setup.

**Windsurf**
1. Copy the skill folder into your project.
2. Reference `SKILL.md` from your `.windsurfrules` file at the project root.

</details>

## Going further after setup

Once the skill has your editor up and running, connect the **[Context7](https://context7.com/) MCP server** to help your agent take the configuration deeper. Instead of leaning on training data, the agent can pull TinyMCE's live documentation and tailor the editor to what you actually need — wiring up additional plugins, adjusting toolbars and menus, setting up callbacks, and refining behavior against the current API surface.

Point Context7 at the TinyMCE docs library (`tinymce/docs`) to give the agent semantic search over the latest plugin pages, config options, and code examples: <https://context7.com/tinymce/tinymce-docs>.

You can also fetch AI-optimized docs directly at:
* <https://www.tiny.cloud/docs/llms.txt>
* <https://www.tiny.cloud/docs/llms-full.txt>

## Contributing

Skills are maintained by the TinyMCE team. To request a new skill or report an issue, [open a GitHub issue](https://github.com/tinymce/tinymce-ai-skills/issues).
