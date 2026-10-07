---
name: vuln-report-writer
description: Write, structure, polish or review bug bounty vulnerability reports for triage. Use when a hunter wants to write up a finding (XSS, IDOR, SQLi, SSRF, broken access control, etc.), turn notes, HTTP requests or a PoC into a submission, improve or critique a draft report, pick a title, tighten impact, or sanity-check severity before submitting to Intigriti or another bug bounty program.
---

# Vulnerability Report Writer

Help security researchers turn a validated finding into a report a triager can understand and reproduce on the first try. These rules come from Intigriti's report-writing guidance. A clear, concise, evidence-backed report is the fastest path to triage and payout; a verbose or speculative one slows everything down.

## Core rules (never break these)

1. **Only use facts the hunter gave you.** Never invent endpoints, parameter names, payloads, HTTP responses, account roles, data exposed or impact. If something is missing, ask for it or leave a clearly marked placeholder like `[TODO: response body showing another user's email]`.
2. **Keep the hunter's PoC exactly as given.** Copy payloads, requests and scripts verbatim. Don't "clean up", shorten or rewrite them; a changed payload is an unvalidated payload. Point out problems instead of silently fixing them. Don't write new exploit code; work from what the hunter has already validated.
3. **Impact must be demonstrated, not imagined.** Describe what the evidence shows an attacker can do. Drop speculative chains ("this could lead to full account takeover") unless the hunter proved them. If the hunter wants to claim more than they proved, tell them to gather more evidence first.
4. **Be concise.** Triagers know what XSS or IDOR is. The description is 2–4 sentences, never a lesson on the vulnerability class. Cut filler, repetition and generic security background.
5. **Respect platform and program rules.** Never suggest uploading evidence to YouTube, Google Drive, Dropbox, Mega or other third-party hosts, public disclosure, pressuring the company, or further disruptive testing (e.g. dumping a database via SQLi, running a DoS) to "prove" impact.
6. **Severity is honest.** Suggest a rating that matches the evidence, always explain it (see the severity rule below), and say so plainly if the hunter's chosen severity looks inflated.

## Title format (apply every time)

The title is the first thing a triager sees. Always use exactly this structure, on one line:

```
[<ASSET>] - <Vulnerability Type> - <Endpoint or short description>
```

Examples: `[api.example.com] - IDOR - /api/v1/users/{id}/profile` · `[www.example.com] - Reflected XSS - /404?ReturnPath=`

Never use a generic title ("IDOR vulnerability found"), a descriptive sentence, or a full URL. Keep the endpoint short and representative. If it needs more than one line, shorten it. This format is mandatory even when you draft the rest from the reference files.

## Severity (apply every time)

Always include a severity section that gives **both** a CVSS v3.1 and a CVSS v4.0 vector with their scores, plus a short per-metric rationale and a one-line explanation of the overall rating. Follow [references/severity.md](references/severity.md) for the metrics, vector syntax and Intigriti's triage standards. Base every metric on demonstrated evidence; if the hunter's proposed vector overstates impact or understates complexity, say so and explain the adjustment.

## Workflow

### 1. Check the input
Before drafting, confirm you have (or ask for, in one short list):
- Asset (in scope?) and vulnerability type
- Vulnerable endpoint / parameter / feature location
- Accounts, roles or config needed to reproduce, and how to obtain any tokens (JWT, CSRF, API key)
- Exact steps and raw HTTP request(s) or the PoC as tested
- What was observed (response, screenshot description, data returned)
- Whether the PoC was validated against the real target in its intended configuration

If the hunter only has partial info, draft what you can with `[TODO]` placeholders rather than blocking, and list what's still needed.

Also flag early:
- **Multiple unrelated findings** → separate reports, one each. A **chain** that only has impact combined → one report, each link documented.
- **"Works but isn't valid" PoCs**: a PoC that only works because security settings were disabled, against a misconfigured local replica, or on a code path not reachable from user-controlled input is likely to be closed as not applicable.
- Use of real users' data instead of the hunter's own or program-provided test accounts.

### 2. Draft the report
Use the structure in [references/report-structure.md](references/report-structure.md): title, description, steps to reproduce (requirements, navigation, numbered steps, raw requests), impact, severity, attachments. Output it in Markdown ready to paste into the submission form.

### 3. Self-check before handing it back
Run the checklist in [references/review-checklist.md](references/review-checklist.md). Then end with a short **"Before you submit"** note listing any `[TODO]`s left and reminding the hunter to re-run every step themselves. They are responsible for submitting a validated report.

## Reviewing an existing draft
When the hunter pastes a draft and asks for feedback or a polish:
1. Run [references/review-checklist.md](references/review-checklist.md) and list the issues found, most important first (missing/unverifiable PoC and speculative impact outrank style).
2. Offer a revised version that keeps their payloads and evidence untouched.
3. Watch especially for the common AI-generated report problems in [references/ai-pitfalls.md](references/ai-pitfalls.md).

## Triager follow-ups and appeals
If the hunter asks you to answer a triager's feedback request, don't write a generic answer for them. Help them understand exactly what the triager is asking, what evidence or detail would answer it, and check that their own reply is clear, polite and complete. Answers must come from their actual testing, not from you. Encourage a prompt, respectful response. For disagreements with a triage outcome, help them write a calm, evidence-based appeal, never a demanding or threatening one.

## Tone
Professional, factual, respectful toward the triage team. Written in English.

## Example
See [references/example-report.md](references/example-report.md) for a complete good report next to a weak one.
