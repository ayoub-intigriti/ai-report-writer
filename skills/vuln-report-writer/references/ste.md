# Simplified Technical English (ASD-STE100)

Write all **prose** in the report in ASD-STE100 Simplified Technical English: short, unambiguous, one idea at a time. This makes reports easy for triagers to read, including non-native English speakers.

## Where STE applies
Apply STE to the writing you generate: the **description**, **steps**, **observed result**, **impact**, and **recommended solution**.

**Do NOT alter** (copy verbatim, STE does not apply): raw HTTP/TCP requests and payloads, scripts, CVSS vectors, the CWE label, the endpoint string, URLs and filenames, and the fixed AI-disclosure sentence. Never reword a payload to "simplify" it.

## Rules

**Sentences**
- Procedural sentences (steps, instructions): **20 words maximum**.
- Descriptive sentences (description, impact, observations): **25 words maximum**.
- One instruction per sentence. One topic per sentence.
- Paragraphs: **6 sentences maximum**, one topic each.

**Voice and structure**
- Use the **active voice** ("The server returns the invoice", not "The invoice is returned").
- Use passive only in descriptive text when the actor is unknown.
- Do not omit the verb, subject, or article to shorten a sentence — keep "the", "a", "an".
- Start each instruction with a command verb ("Send the request", "Log in as Victim").
- Use vertical (numbered or bulleted) lists for anything with several parts.

**Verbs**
- Use only: infinitive, imperative, simple present, simple past, simple future.
- Do not stack auxiliary verbs into complex tenses (no "will have been", "has been being").
- Use a past participle only as an adjective ("the returned data").
- Use an "-ing" form only inside a technical name, not as a verb ("the testing account" is fine; "by changing the ID" → "when you change the ID").

**Words**
- One word, one meaning; prefer the simplest word that is accurate.
- Keep a multi-word noun to **three words maximum**.
- No slang, idioms, or filler. Keep security terms (IDOR, token, endpoint) — those are technical vocabulary.
- Be consistent: use the same word for the same thing throughout (don't switch between "attacker" and "adversary").

## Quick before/after
- Before: "By manipulating the identifier, an attacker would be able to retrieve invoices belonging to other organizations that should not have been accessible."
- After (STE): "Change the invoice ID in the request. The server returns an invoice from a different organization. The attacker must not have access to it."
