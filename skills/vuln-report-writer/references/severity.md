# Severity

Give an honest rating backed by the evidence, and always **explain** it. Provide both a CVSS v3.1 and a CVSS v4.0 vector with scores, a short rationale per base metric, and one line on the overall rating. If the program sets its own severity scheme or scope, that overrides CVSS.

## Always cite the clause you rely on

Whenever you state something as an Intigriti standard or ruling — a multi-tenant scoring rule, a pre-set severity, the PoC requirement — **cite the specific clause**: its section number and name plus the link, e.g. "Triage Standards §6.1 (Open redirect) — https://kb.intigriti.com/en/articles/10335710-intigriti-triage-standards". Never assert a policy claim without the reference, so the researcher can verify it. (The page has no per-heading anchors, so cite the section number; don't invent anchor fragments.) Relevant sections: §1.1 PoC-based scoring, §1.2 vulnerability-type scoring, §3.1.6 Confidentiality / §3.1.7 Integrity (multi-tenant notes), §6 Rulings and exceptions.

## How to present it

```markdown
## Severity
**<Rating> — <v3.1 score>** · `CVSS:3.1/AV:.../A:.`
**<Rating> — <v4.0 score>** · `CVSS:4.0/AV:.../SA:.`

| Metric | v3.1 | v4.0 | Why |
|---|---|---|---|
| Attack Vector | N | N | Exploitable remotely over the API |
| ... | | | |

<One sentence explaining the overall rating in the program's context.>
```

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
