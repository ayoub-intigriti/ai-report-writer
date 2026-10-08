# Report structure

Every report has six parts. Keep each one as short as it can be while still letting a triager reproduce the issue without guesswork.

## 1. Title

Format:

```
[<ASSET>] - <Vulnerability Type> - <Endpoint or short description>
```

Good:
- `[www.example.com] - Reflected XSS - /404?ReturnPath=`
- `[api.example.com] - IDOR - /api/v1/users/{id}/profile`
- `[app.example.com] - SQL Injection - /search?query=`

Avoid:
- Generic titles: "XSS vulnerability found", "Security issue on login page"
- Full URLs in the title; use a short, representative endpoint
- Titles longer than one line

## 2. Description

2–4 sentences:
1. What behavior was observed and the vulnerability type.
2. Why it shouldn't be possible / how it can be abused.

Don't explain how the vulnerability class works in general. Just enough context to follow the steps.

## 3. Steps to reproduce (most important section)

The triager should reproduce the finding by following the steps exactly, with no extra context. Assume they're opening the target for the first time.

**Requirements** (list first, if any):
- Accounts and roles needed (e.g. two standard accounts, Attacker and Victim; or admin + standard user)
- Any special configuration
- How to obtain required tokens (JWT, anti-CSRF token, API key)

**Navigation**: give the exact UI path, e.g. "Navigate to Settings > Integrations > API Keys".

**Steps**: numbered, factually correct, in exact execution order. Include parameter values, headers and payloads that matter. No unnecessary steps or verbose logs. Give the **simplest path to reproduction** — Intigriti's Triage Standards require "the simplest possible demonstration that proves the vulnerability's exploitability and impact beyond reasonable doubt." Validate the steps yourself first, ideally with fresh test accounts or a clean instance.

**Raw requests and scripts**:
- Always include the raw HTTP (or TCP) request(s), even when a script is attached, so the finding can be validated without running the script.
- Scripts should have minimal dependencies and nothing from unofficial or untrusted package sources.

**Evidence**: screenshots for every key step (they keep the report valid if the issue later stops reproducing). Video PoCs are encouraged and some programs require them.

## 4. Impact

Short and specific: what an attacker can realistically do, based on the evidence.
- Reflected XSS: can they steal session cookies (are they HttpOnly?), act on behalf of the victim, redirect them?
- IDOR: exactly what data is exposed or which actions can be taken on another user's behalf?

Never overstate. No speculative attack scenarios. Severity is mostly determined by the evidence provided.

Most programs prohibit disruptive testing, so don't fully exploit SQL injection or DoS issues to prove impact. Demonstrate the minimum that proves the issue (e.g. a version string or boolean/time-based difference) and state that you stopped there per program rules. In that case the triager scores on potential impact rather than the evidence, and the company may adjust severity once it has gathered more information.

## 5. Severity

Honest and evidence-based, and always explained. Provide both a CVSS v3.1 and a CVSS v4.0 vector with scores, a short per-metric rationale, and one line on the overall rating — see [severity.md](severity.md) for the metrics and Intigriti's triage standards. Check the program's own severity guidelines first; they override CVSS. Inflating Medium/Low findings to Critical doesn't raise the bounty; it slows triage and erodes trust. Accurate severities build credibility with triage teams.

## 6. Attachments

Everything in one place, uploaded to the platform: PoC scripts, every screenshot referenced in the steps, video where applicable, raw HTTP requests.

Never use YouTube, Dropbox, Google Drive, Mega.nz or similar third-party hosts; it's against Intigriti's Community Code of Conduct because evidence can contain sensitive data. If a file can't be uploaded to the platform due to technical limits, put it in a password-protected ZIP in a secure location and include the password in the report. When in doubt, ask the platform first.

## Template

~~~markdown
**Title:** [<asset>] - <vulnerability type> - <endpoint>

## Description
<What you observed and the vulnerability type. Why it shouldn't be possible.>

## Steps to reproduce

**Requirements**
- <accounts / roles / config>
- <how to obtain any required token>

**Steps**
1. Navigate to <UI path>.
2. <exact action, with parameter values>
3. <...>

**Request**
```http
<raw HTTP request, verbatim>
```

**Observed result**
<what the response/screen shows; reference screenshot names>

## Impact
<What an attacker can demonstrably do, based on the evidence above.>

## Severity
**<Rating> — <score>** · `CVSS:3.1/<vector>`
**<Rating> — <score>** · `CVSS:4.0/<vector>`
<one sentence why, in the program's context>

## Attachments
- <screenshot-1.png: description>
- <poc-video.mp4: description>

---
*This report was generated by Intigriti AI Report Writer for Claude.*
~~~

(Replace "Claude" with whichever assistant is running the skill: Claude, ChatGPT or Codex.)
