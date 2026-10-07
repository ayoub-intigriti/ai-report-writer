# Common problems in AI-written reports

Triage teams see a growing number of fully LLM-generated submissions. Used carelessly, AI shifts the work onto the triager instead of removing it. Avoid these six problems, both when writing and when reviewing.

1. **Lengthy paragraphs.** LLMs default to verbose output: long explanations of the vulnerability class, repeated points, background nobody asked for. Every extra paragraph is time the triager spends reading, and more room for hallucinated details. Keep only what's needed, plus evidence.

2. **Altered or placeholder PoCs.** An LLM may "improve" a working payload or insert a generic one, producing an unvalidated submission. Keep the hunter's PoC verbatim. Also beware PoCs that run but don't prove anything: e.g. a firewall bypass where the firewall rules were never enabled, or a vulnerable code snippet that isn't reachable through user-controlled input in a production-like setup. Those are often closed as not applicable.

3. **Incorrect reproduction steps.** Without real context, an LLM guesses at navigation, parameters and order of operations. A wrong step means the triager fails to reproduce, or has to come back with questions. Steps must come from what the hunter actually did, and the hunter must re-run them before submitting.

4. **Advice that breaks platform rules.** LLMs commonly suggest linking a video on an external host. Intigriti's Community Code of Conduct disallows external hosting and file-sharing services. If platform upload isn't possible, use a password-protected ZIP in a secure location with the password in the report.

5. **Speculative attack vectors.** LLMs are good at making things sound like they work. Triagers can't act on assumptions; they follow the platform's triage standards and program rules. Speculative impact slows triage or leads to an incorrect severity. If it isn't proven, gather more evidence first.

6. **AI-generated replies to feedback requests.** When a triager asks for more information, a chatbot answer can be wrong, off-topic or miss the question. The hunter should answer in their own words from their actual testing; AI can help them understand the question and check the reply for clarity.

Human oversight is mandatory: never assume an AI or tool has validated the vulnerability.
