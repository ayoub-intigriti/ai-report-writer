# Web flavor — HTTP web-app findings

Use this module together with [report-structure.md](report-structure.md) (shared skeleton) and [severity.md](severity.md) (severity). Read it when the vulnerability is exploited **over HTTP against a web application**. It defines only the parts that differ from product findings; everything else comes from the skeleton.

## Field 3 — Endpoint / vulnerable component
- **Host + path, no protocol** (no `https://`): `api.example.com/api/v1/users/<id>/profile`.
- Positional/path parameters use **`<>` not `{}`**: `/programs/<program_uuid>`.
- Multiple endpoints → separate with commas.
- Injection point in a request-body parameter or a header → pipe syntax: `www.example.com/login.php | returnUrl=<PAYLOAD>`.
- **GraphQL / RPC module**: the HTTP path alone is not enough — include the **operation/method name and the vulnerable parameter**. Examples: `api.example.com/graphql | mutation updateUserRole | role=<PAYLOAD>`, `api.example.com/rpc | Account.setEmail | email=<PAYLOAD>`.

## Environment
Web findings do **not** use the `### Environment` block. Omit it.

## PoC — requests and commands
- Put each **raw HTTP request** inline, directly under the step that sends it (not in a trailing section). A multi-request chain reads step, request, step, request.
- Include any commands the researcher must run to prepare a value, e.g. `openssl` to generate a base64 string or a secret, under their step too. These are common in web PoCs.
- Always include the raw request even when a script is attached. Keep payloads verbatim. Never host evidence externally.

## Example (web-specific fields)
```
Endpoint:  app.example.com/api/v2/invoices/<id>/pdf
CWE:       CWE-639 Insecure Direct Object Reference (Broken Access Control)
```
The full worked web report is in [example-report.md](example-report.md).
