# Review checklist

Run this on every report you draft or review. Report problems most important first.

## Blocking issues (fix before submitting)
- [ ] Asset is in scope and the vulnerability class isn't excluded by the program
- [ ] One vulnerability per report (or one documented chain)
- [ ] PoC is the hunter's validated PoC, unchanged; no placeholder or untested payloads
- [ ] PoC works against the target in its intended configuration (not a misconfigured replica, not with protections disabled, reachable from user input)
- [ ] Steps are complete, numbered, in order, with every parameter value and header that matters
- [ ] Requirements listed first: accounts/roles, config, how to get tokens
- [ ] Raw HTTP/TCP request included, not just a script
- [ ] Impact only claims what the evidence shows; no speculative scenarios
- [ ] Testing used the hunter's own or program-provided test accounts
- [ ] No evidence hosted on third-party services (YouTube, Drive, Dropbox, Mega, ...)
- [ ] No disruptive testing beyond what's needed to prove the issue
- [ ] Steps give the simplest path to reproduction
- [ ] Live credentials and PII redacted in requests and screenshots
- [ ] AI-assistance disclosure line included

## Quality issues
- [ ] Title follows `[asset] - type - endpoint`, one line, no full URL
- [ ] Description is 2–4 sentences, no vulnerability-class lecture
- [ ] Navigation path given for features deep in the app
- [ ] No unnecessary steps, filler or verbose logs
- [ ] Severity matches the evidence, the program's guidelines, and any standing ruling; both CVSS v3.1 and v4.0 vectors given and explained
- [ ] Every Intigriti standard/ruling claim cites its clause (section number + name + link)
- [ ] Output follows the submission-form order; endpoint derived from the PoC; vulnerability type given as a CWE
- [ ] Recommended solution is 1–2 sentences; researcher reminded about mandatory submission questions
- [ ] Screenshots referenced in the steps are listed in attachments
- [ ] Script dependencies are minimal and from official sources
- [ ] English, professional and respectful tone
- [ ] No leftover `[TODO]` placeholders (or they're called out to the hunter)
