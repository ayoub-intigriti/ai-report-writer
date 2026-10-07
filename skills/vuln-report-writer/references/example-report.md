# Example: weak vs. good report

Both describe the same (fictional) broken access control issue on `app.example.com`.

## Weak report (don't do this)

> **Title:** Critical Security Vulnerability Found in Invoice System
>
> **Description:** Insecure Direct Object Reference (IDOR) is a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. IDOR vulnerabilities were popularized in the OWASP Top 10 and remain one of the most common issues in modern web applications... *(three more paragraphs)*
>
> **Steps:** 1. Log in. 2. Go to invoices. 3. Change the ID to `<VICTIM_ID>` using this payload: `GET /api/invoice?id=1 OR 1=1`
>
> **Impact:** An attacker could download every invoice, use the data to take over accounts, perform phishing at scale and potentially compromise the entire company.
>
> **Video:** https://youtube.com/watch?v=...
>
> **Severity:** Critical

Problems: generic title; lecture instead of description; vague steps with a placeholder and an unrelated, untested payload; no requirements or raw request; speculative impact; evidence on YouTube (Code of Conduct violation); inflated severity.

## Good report

~~~markdown
**Title:** [app.example.com] - IDOR - /api/v2/invoices/{id}/pdf

## Description
The invoice PDF endpoint returns any invoice by numeric ID without checking that it belongs to the authenticated user's organization. A user can download other organizations' invoices by changing the ID.

## Steps to reproduce

**Requirements**
- Two accounts in two different organizations: Attacker (attacker@test.example) and Victim (victim@test.example). Both are free-tier accounts registered via /signup.

**Steps**
1. Log in as Victim and navigate to Billing > Invoices. Note the invoice ID of the newest invoice (here: `48213`).
2. Log out, log in as Attacker, and navigate to Billing > Invoices.
3. Click "Download PDF" on any of Attacker's own invoices and intercept the request.
4. Replace the invoice ID in the path with Victim's ID `48213` and send the request:

```http
GET /api/v2/invoices/48213/pdf HTTP/2
Host: app.example.com
Cookie: session=<Attacker's session cookie>
```

**Observed result**
The server responds `200 OK` with Victim's invoice PDF, showing Victim's organization name, billing address and line items (screenshot-2.png).

## Impact
Any authenticated user can download invoices of other organizations, exposing organization name, billing address, purchased plan and amounts. Invoice IDs are sequential, so other invoices can be retrieved by changing the ID. Testing was limited to the two test accounts above.

## Severity
Medium (CVSS 3.1: AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:N/A:N, 4.3): read-only exposure of business billing data, no personal payment card data shown.

## Attachments
- screenshot-1.png: Victim's invoice list showing ID 48213
- screenshot-2.png: Attacker's response containing Victim's invoice
- poc-video.mp4: full reproduction (uploaded to the platform)
~~~
