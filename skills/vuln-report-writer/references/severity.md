# Severity

Give an honest rating backed by the evidence, and always **explain** it. Provide both a CVSS v3.1 and a CVSS v4.0 vector with scores, a short rationale per base metric, and one line on the overall rating. If the program sets its own severity scheme or scope, that overrides CVSS.

## Score the highest impact the evidence supports, then flag caveats

Default to the **maximum impact the PoC actually demonstrates** — Intigriti's PoC-based scoring considers the greatest demonstrated impact (Triage Standards §1.1). Do **not** pre-apply downgrades (multi-tenant rulings, AC complexity, §6 rulings) into the score. Score the high case, then add a short **"a triager may score this lower if…"** caveat naming the factor and citing the clause, so the researcher knows the risk without under-selling the finding. This is the opposite of scoring conservatively and saying "may need upgrading."

Example: for an access-control bypass returning private data, score Confidentiality **High**, then note: *"if the response only exposes data already public, a triager may drop Confidentiality to Low per §3.1.6."* Never invent impact beyond the evidence (rule 3) — "highest supported by evidence" is not "speculative."

## Metric conventions (apply these automatically)

These resolve the metrics researchers most often get wrong. They follow the CVSS definitions (https://www.first.org/cvss/v3.1/specification-document, https://www.first.org/cvss/specification-document).

**Privileges Required (PR)** — set by how the attacker obtains the account, not by whether one is used:
- **PR:N** — no privilege the target controls is needed. If the app allows **open self-registration**, anyone can create an account, so holding a normal account is not a real barrier → PR:N. (CVSS: the attacker is "unauthorized prior to attack".)
- **PR:L** — privileges the attacker **cannot self-obtain**: membership of a tenant they must be **invited** to, or a basic role granted by someone else.
- **PR:H** — **full administrative** control over the vulnerable component is required.

**Attack Complexity (AC) — unguessable values:**
- Exploitation needs an ID, token or value that is **not guessable** (e.g. a UUID or random token) and the researcher gives **no evidence** of how an attacker obtains it → **AC:H** (CVSS AC:H = the attacker must gather target-specific information that is not readily available).
- The value is **predictable** (sequential/numeric ID, enumerable) → **AC:L**.
- In **CVSS v4.0** this precondition is an Attack Requirement, not complexity: use **AT:P** for the unguessable-with-no-known-source case and **AT:N** when predictable; keep AC:L unless there is genuine execution complexity.

## Do not hand-compute the number
Output the **vector and the qualitative rating**; the exact numeric score comes from the official FIRST calculator (the researcher pastes the vector in). Hand-calculating, especially for v4.0's lookup-table scoring, is error-prone, so don't print a fabricated number.

## Always cite the clause you rely on

Whenever you state something as an Intigriti standard or ruling — a multi-tenant scoring rule, a pre-set severity, the PoC requirement — **cite the specific clause**: its section number and name plus the link, e.g. "Triage Standards §6.1 (Open redirect) — https://kb.intigriti.com/en/articles/10335710-intigriti-triage-standards". Never assert a policy claim without the reference, so the researcher can verify it. (The page has no per-heading anchors, so cite the section number; don't invent anchor fragments.) Relevant sections: §1.1 PoC-based scoring, §1.2 vulnerability-type scoring, §3.1.6 Confidentiality / §3.1.7 Integrity (multi-tenant notes), §6 Rulings and exceptions.

## How to present it

```markdown
## Severity
`CVSS:3.1/AV:.../A:.` → <Rating>
`CVSS:4.0/AV:.../SA:.` → <Rating>
(Paste each vector into the FIRST calculator for the exact score.)

| Metric | v3.1 | v4.0 | Why |
|---|---|---|---|
| Attack Vector | N | N | Exploitable remotely over the API |
| Attack Complexity | L/H | L/H | ... |
| Attack Requirements | — | N/P | v4.0 only |
| Privileges Required | N/L/H | N/L/H | ... |
| User Interaction | N/R | N/P/A | ... |
| Scope | U/C | — | v3.1 only |
| Confidentiality (Vuln. System) | C | VC | ... |
| Integrity (Vuln. System) | I | VI | ... |
| Availability (Vuln. System) | A | VA | ... |
| Subsequent Confidentiality | — | SC | v4.0 only |
| Subsequent Integrity | — | SI | v4.0 only |
| Subsequent Availability | — | SA | v4.0 only |

<One sentence on the overall rating, then any "a triager may score lower if… (§clause)" caveats.>
```

Always include the three Subsequent-System rows (SC/SI/SA); they are v4.0-only, so mark the v3.1 column `—`. Likewise Scope is v3.1-only and Attack Requirements v4.0-only.

Only include a metric value you can justify from the evidence. If the hunter's proposed vector overstates impact or understates complexity, adjust it and say why.

## CVSS v3.1 base metrics

Vector prefix `CVSS:3.1/`. All base metrics mandatory, in this order: AV, AC, PR, UI, S, C, I, A.

| Metric | Abbr | Values |
|---|---|---|
| Attack Vector | AV | N (Network), A (Adjacent), L (Local), P (Physical) |
| Attack Complexity | AC | L (Low), H (High) |
| Privileges Required | PR | N (None), L (Low), H (High) |
| User Interaction | UI | N (None), R (Required) |
| Scope | S | U (Unchanged), C (Changed) |
| Confidentiality | C | H (High), L (Low), N (None) |
| Integrity | I | H, L, N |
| Availability | A | H, L, N |

Rating bands: None 0.0 · Low 0.1–3.9 · Medium 4.0–6.9 · High 7.0–8.9 · Critical 9.0–10.0.

## CVSS v4.0 base metrics

Vector prefix `CVSS:4.0/`. Base metrics in this order: AV, AC, AT, PR, UI, VC, VI, VA, SC, SI, SA. Abbreviations are case-sensitive. v4.0 drops Scope and splits impact into the **Vulnerable System** (VC/VI/VA) and **Subsequent System** (SC/SI/SA).

| Metric | Abbr | Values |
|---|---|---|
| Attack Vector | AV | N, A, L, P |
| Attack Complexity | AC | L (Low), H (High) |
| Attack Requirements | AT | N (None), P (Present) |
| Privileges Required | PR | N (None), L (Low), H (High) |
| User Interaction | UI | N (None), P (Passive), A (Active) |
| Vulnerable System Confidentiality | VC | H, L, N |
| Vulnerable System Integrity | VI | H, L, N |
| Vulnerable System Availability | VA | H, L, N |
| Subsequent System Confidentiality | SC | H, L, N |
| Subsequent System Integrity | SI | H, L, N |
| Subsequent System Availability | SA | H, L, N |

Nomenclature: **CVSS-B** base only, **CVSS-BT** base+threat, **CVSS-BE** base+environmental, **CVSS-BTE** all. A report normally quotes the base score (CVSS-B). Rating bands are the same as v3.1.

### Mapping notes (v3.1 → v4.0)
- `S` (Scope) is gone. Impact that in v3.1 you expressed via Scope:Changed now goes into the **Subsequent System** metrics (SC/SI/SA) — effects beyond the vulnerable component itself.
- The v3.1 "AC:H because a precondition must be met" often splits in v4.0: genuine execution-time complexity stays in **AC:H**, while a required precondition the attacker must first obtain (a token, a specific config, a leaked identifier) belongs in **AT:P**.
- v3.1 `UI:R` becomes either **UI:P** (passive, the victim just has to be using the app) or **UI:A** (active, the victim must click/act).

## Intigriti triage standards

(From Intigriti's Triage Standards, effective 6 Jan 2025, which replaced the older contextual CVSS guidelines.)

- Triage assigns the final severity; the researcher's proposed severity is a starting point. Submit an honest rating, not an inflated one.
- **Default scoring is proof-of-concept based**: severity reflects the impact the PoC actually demonstrates, up to the maximum demonstrated impact (e.g. XSS shown escalating to RCE). A program owner may instead score by **vulnerability type**, judging the immediate effect and ignoring later escalation and mitigating factors; that choice is theirs and isn't mediated.
- **Platform severity ≠ company risk.** The score reflects demonstrated technical impact, not the asset's business importance. Program descriptions, scope and any custom scoring the program sets override the platform standard.
- **Multi-tenant systems** (§3.1.6 / §3.1.7): a partial access-control bypass is usually scored Low confidentiality/integrity unless impact spans a whole tenant or the data is a significant risk to the core business. Cite this clause when you apply it.
- **Speculative issues** (test environments, unused/unreachable code, low-entropy secrets with no shown exploit) are judged case by case and are often marked **Undecided** — provide evidence rather than assertion.
- **Chains** can be scored on combined impact when unique and unreported. Don't hoard a finding more than 48 hours. A duplicate is only reassessed if it shows additional impact.
- The required PoC is *"the simplest possible demonstration that proves the vulnerability's exploitability and impact beyond reasonable doubt."* Score against what that demonstration shows.

### Rulings and exceptions (pre-set severities, §6)

Always read the program description first — some programs set their own severities for certain vulnerability types, and those prevail. Intigriti's standing rulings (§6, these can change, so re-check the live standards):

| § | Vulnerability type | Default | Exception |
|---|---|---|---|
| 6.1 | Open redirect (external redirect only, no other purpose) | Low | — |
| 6.2 | Content / HTML / CSS injection | Low | Unless critical confidential data can be stolen (e.g. dangling markup) |
| 6.3 | Broken link hijacking | Low | Integrity counts if a server-side component uses the URL to an attacker's benefit |
| 6.4 | Debug / path / limited source-code disclosure | Low | Unless the PoC shows the data can be leveraged |
| 6.5 | Invalidation (purging caches/buffers) | Low | If it creates business risk |
| 6.6 | WAF bypass | Low | Unless further impact on the protected app is shown |
| 6.7 | Cookie-bombing DoS | Low | — |

If the finding is one of these, score it accordingly and don't inflate it; note the ruling **with its section number and link** in the report so the triager sees you've accounted for it. (Section numbers are indicative of order; confirm against the live page.)

Always double-check against the live standards and the specific program's rules before submitting: https://kb.intigriti.com/en/articles/10335710-intigriti-triage-standards
