# Intigriti AI Report Writer

Intigriti AI Report Writer is a skill for Claude and ChatGPT to help bug bounty hunters write clear, concise vulnerability reports that triagers can reproduce on the first try. This plugin is also capable of flagging common pitfalls before submitting your report.

It's built on Intigriti's report-writing guidance:
- [How to write a good report​​​​‌](https://www.intigriti.com/researchers/hackademy/guides/how-to-write-a-good-report)
- [How to use AI for improved vulnerability report writing](https://www.intigriti.com/researchers/blog/hacking-tools/vulnerability-report-writing-with-ai)
- [Bug Bounty Starter Kit](https://www.intigriti.com/bug-bounty-starter-kit)

## What it does

- Turns your notes, HTTP requests and PoC into a structured report: title, description, steps to reproduce, impact, severity and attachments
- Keeps your payloads and requests exactly as you tested them
- Keeps impact to what your evidence proves, and suggests an honest severity
- Flags missing information instead of making it up
- Reviews an existing draft and lists what to fix before you submit
- Follows Intigriti's Community Code of Conduct (e.g. no third-party evidence hosting)

> [!IMPORTANT]
> You stay responsible for validating your finding and re-running every step before you submit.

## Install

### Claude Code

```
/plugin marketplace add intigriti/ai-report-writer
/plugin install ai-report-writer@intigriti
```

### Claude app (claude.ai, desktop, mobile)

Download [`skills/vuln-report-writer`](skills/vuln-report-writer) as a ZIP and upload it in Claude's skills settings.

### ChatGPT and Codex

```
codex plugin marketplace add intigriti/ai-report-writer
```

Then open the Plugins Directory in the ChatGPT desktop app, pick the **Intigriti** marketplace and install **Intigriti Report Writer**.

### Other agents

The skill follows the open [Agent Skills](https://agentskills.io) format, so the `skills/vuln-report-writer` folder works in any agent that supports `SKILL.md`.

## Usage

Just describe your finding. The skill activates automatically:

> Write up this IDOR. Here's the request and response: ...

> Review my XSS report before I submit it: ...

> Is this a medium or a high?

## Repository layout

```
├── plugin.json                    # Portable Agent Plugins manifest (ChatGPT/Codex)
├── .claude-plugin/
│   ├── plugin.json                # Claude Code manifest
│   └── marketplace.json           # Claude Code marketplace
├── .agents/plugins/
│   └── marketplace.json           # ChatGPT/Codex marketplace
└── skills/vuln-report-writer/
    ├── SKILL.md                   # Core instructions
    └── references/
        ├── report-structure.md    # Section-by-section guide and template
        ├── review-checklist.md    # Pre-submission checklist
        ├── ai-pitfalls.md         # Common problems in AI-written reports
        └── example-report.md      # Weak vs. good report
```

To update the guidance, edit the files in `skills/vuln-report-writer/` and bump `version` in both `plugin.json` files.
