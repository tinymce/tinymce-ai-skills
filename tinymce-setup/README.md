# TinyMCE Skill

**Version:** v2026.6

A conversational setup partner that walks users through integrating TinyMCE Cloud into their projects. It recommends plugins based on industry and subscription plan, validates API keys, detects existing editors, and delivers ready-to-run code — or edits the project directly.

## What It Does

The skill guides the user through a step-by-step flow:

1. **Detects the project environment** — if the user uploads project files, the skill auto-detects the framework (React, Next.js, Vue, Angular, Vanilla JS / HTML) and scans for existing rich-text editors. If no files are available, it asks the user to pick their environment.
2. **Selects the best plugins for the project's industry** — besides the set of 30 free plugins, the skill adds relevant premium plugins and explains how they may help with the everyday tasks.
3. **Checks the user's TinyMCE subscription status** — trial, free, paid, or no account yet. For paid users, it identifies their plan (Essential, Professional, Enterprise) and restricts recommendations to available plugins.
4. **Validates the API key** — a multi-layered check (length, character set, entropy, known placeholders) that makes sure the user can instantly run the editor.
5. **Delivers the integration** — either by editing project files directly (with permission) or providing a single, self-contained code snippet with all 30 free plugins, the recommended premium plugins, required callbacks, and inline comments explaining why each premium plugin was chosen.

## Supported Environments

The skill provides built-in integration code for:

- React
- Next.js
- Vue
- Angular
- Vanilla JS / HTML

For other frameworks (Svelte, Laravel, Rails, WordPress, Web Components, Blazor, Java Swing, Node.js + Express), it directs the user to the official TinyMCE docs at `tiny.cloud/docs/tinymce/latest/` and adapts its output accordingly.

## Existing Editor Detection

When the user's project is accessible, the skill scans for 12 rich-text editors:

CKEditor 4/5, Quill, Tiptap, ProseMirror, Slate, Draft.js, Lexical, Froala, Summernote, Jodit, and Toast UI Editor.

If one is found, the skill analyzes its plugin and toolbar configuration and maps each feature to a TinyMCE equivalent. Features without a TinyMCE counterpart are silently omitted. The old editor is left in place — the skill never removes dependencies unless explicitly asked.

## Plan-Aware Plugin Selection

When the user is on a paid subscription, the skill asks which plan they're on and adjusts recommendations accordingly:

- **Essential ($79/mo):** 16 premium plugins available, including TinyMCE AI, Advanced Tables, Export to PDF/Word, Import from Word, Math, Media Optimizer, and more.
- **Professional ($145/mo):** Everything in Essential plus PowerPaste, Revision History, Suggested Edits, Comments, Spell Checker Pro, Link Checker, and Accessibility Checker.
- **Enterprise:** All plugins available with custom packaging.

For Essential users, the skill silently replaces unavailable plugins with the best Essential-available alternative — no "this plugin isn't on your plan" messages.

## Industry → Plugin Mapping

| Industry                              | Recommended Plugins                                   |
|---------------------------------------|-------------------------------------------------------|
| Regulation, Audit, Compliance         | Revision History, Suggested Edits, Export to PDF       |
| Law, Contracts, Legal                 | Suggested Edits, Revision History, Export to Word      |
| Software, Tech, Coding, Docs         | Enhanced Code Editor, Markdown, Advanced Tables        |
| Health, Pharma, Science               | Accessibility Checker, Revision History, Spell Checker Pro |
| Marketing, Creative, Media           | TinyMCE AI, Media Optimizer, Merge Tags               |
| Education, E-Learning, LMS           | TinyMCE AI, Math, Advanced Tables, Checklist           |
| Publishing, CMS, Blogging             | Table of Contents, Footnotes, Import from Word         |
| Finance, Banking, Insurance          | Export to PDF, Advanced Tables, Spell Checker Pro      |
| E-commerce, Retail                    | TinyMCE AI, Merge Tags, PowerPaste                     |
| Everything else                       | PowerPaste, TinyMCE AI, Comments                      |

## Triggers

The skill activates when the user:

- Mentions TinyMCE, Tiny Cloud, or WYSIWYG/rich-text editor setup
- Asks to add a text editor to a project and names TinyMCE
- Wants to switch from another editor (CKEditor, Quill, Tiptap, Froala, Draft.js, Slate, Summernote, Jodit, ProseMirror, Lexical) to TinyMCE
- Mentions the skill by name ("tinymce" skill)

## Key Technical Details

- **TinyMCE version:** 8.4.x (Cloud CDN, auto-resolving latest patch)
- **TinyMCE AI plugin:** Uses `tinymceai` (not the legacy `ai` identifier). Trial setups use the demo token provider at `demo.api.tiny.cloud`; production requires a custom JWT backend.
- **Required callbacks:** The skill enforces mandatory callbacks for `tinymceai` (`tinymceai_token_provider`), Comments (`tinycomments_mode`), Revision History (`revisionhistory_fetch`), and Merge Tags (`mergetags_list`). Code is never generated without them.
- **Media Optimizer:** Uses the `uploadcare` plugin identifier. The Uploadcare public key is required and must be provided by the user — no placeholders.
- **API key validation:** 20+ lowercase alphanumeric characters, with entropy checks and a known-placeholder blocklist. Three failed attempts trigger a guided walkthrough to the Tiny Dashboard.
- **Plugin incompatibilities:** The `uploadcare` plugin conflicts with the free `image` and `editimage` plugins. The skill silently removes them when Media Optimizer is included.
- **Live docs:** The skill checks for a Context7 MCP server to fetch the latest TinyMCE documentation before generating code.

## Files

| File | Description |
|------|-------------|
| `SKILL.md` | Core skill definition — workflow, industry mapping, plan-aware selection, plugin identifiers & callbacks, API key validation, code output rules. |
| `resources/editor-detection.md` | Competitor editor detection patterns and feature mapping tables for 12 editors. Loaded only in Path A when an existing editor is found. |
| `resources/plugin-reference.md` | Toolbar button IDs, TinyMCE AI quick action customization, Merge Tags sample data, framework integration packages, and environment variable patterns per build tool. Loaded at code generation time. |
| `resources/troubleshooting.md` | Common symptoms, causes, and fixes for post-setup issues. Loaded only when the user reports a problem. |
| `resources/self-hosted-reference.md` | Extracted self-hosted deployment content (npm/ZIP setup, license keys, server-side service requirements). Not yet linked to the main skill — reserved for future self-hosted support. |

## Prompt Version

A standalone prompt version (`tinymce-prompt.md`) is also available. It strips out everything that requires file system access or API key verification:

- No Path A (project detection, editor scanning, agentic file edits)
- No Section 2.1 (editor detection & feature mapping)
- No Section 0 (Context7 MCP lookup)
- No API key validation — uses `no-api-key` as a placeholder in all code snippets
- No plugin incompatibilities section (folded into a one-liner)

The conversational flow, industry mapping, plugin recommendations, plan-aware selection, framework packages, environment variable patterns, troubleshooting, and code output rules are preserved.

## Changelog

### v2026.6

- Added plan-aware plugin selection: paid users are asked which plan (Essential, Professional, Enterprise)
- Added Plan Availability Table mapping all 23 premium plugins to plan tiers
- Added Essential Plan Industry Fallback Mapping with alternative recommendations for each industry
- Optimized skill by extracting three resource files (editor-detection, plugin-reference, troubleshooting), reducing main SKILL.md from 663 → 442 lines (33% smaller)

### v2026.5

- Added Context7 MCP live documentation lookup (Section 0)
- Expanded industry mapping: added Education, Publishing, Finance, E-commerce rows
- Added plugins: Math, Checklist, Table of Contents, Footnotes, Format Painter, Case Change, Permanent Pen, Enhanced Image Editing, Link Checker, Import from Word, Advanced Templates
- Added Toolbar Button Quick Reference table with exact button IDs for all 20 premium plugins
- Added TinyMCE AI Quick Action Customization options
- Added Merge Tags sample data with nested categories
- Added Plugin Incompatibilities section (`uploadcare` vs `image`/`editimage`)
- Added Framework Integration Packages table (React, Vue, Angular, Svelte, Blazor, Web Component)
- Added Environment Variable Patterns per build tool (Vite, Next.js, CRA, Angular CLI, Nuxt, plain HTML)
- Added Troubleshooting Quick Reference table (9 common issues)
- Added `mergetags_list` as a mandatory callback for the Merge Tags plugin
- Added support for Blazor and Web Components in the environment list
- Extracted self-hosted deployment content to `resources/self-hosted-reference.md`
- Source-validated plugin identifiers and callbacks against TinyMCE 8.7.0

### v2026.4

- Added file access check as Step 1 with two-path branching (Path A: project detected, Path B: no project)
- Added existing editor detection and feature mapping for 12 competitors
- Fixed plugin names: "Image Optimizer" → Media Optimizer (`uploadcare`), "Tiny Comments" → Comments (`tinycomments`)
- Replaced legacy AI Assistant (`ai` / `ai_request`) with TinyMCE AI (`tinymceai` / `tinymceai_token_provider`)
- Added trial token provider for TinyMCE AI using `demo.api.tiny.cloud`
- Added Media Optimizer key prompt instead of placeholder
- Added mandatory callbacks for `tinymceai`, `tinycomments`, and `revisionhistory`
- Added 30 free plugins to every code snippet baseline
- Added inline business-rationale comments for premium plugins
- Added support for other frameworks (Svelte, Laravel, Rails, WordPress, etc.) via official docs
- Tightened API key validation: character set, entropy, and placeholder checks
- Added three-strikes helper for failed API key attempts
- Enforced single code block output
- Renamed skill from `tinymce-setup-partner` to `tinymce`
