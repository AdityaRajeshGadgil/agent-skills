---
name: vivek-review
description: Run a strict independent ship-readiness review after development work on code, products, documents, configuration, tests, deployments, or generated artifacts. Use before final handoff and when asked for Vivek Review, a ship/no-ship decision, an engineering and product quality review, or a simplicity pass. Send a neutral evidence packet to a fresh reviewer session that uses the model and reasoning effort the user chose for reviews (else the main agent's), then address findings and repeat until an unequivocal YES or a concrete blocker.
---

# Vivek Review

Ask whether, applying the highest standards of product judgment and engineering, you would be proud to ship the completed change exactly as it stands. Return `YES` only if you would change nothing. Return `NO` for every issue you would fix, including small polish issues.

## Main agent

1. Finish the requested work and run the applicable checks.
2. Assemble a neutral review packet with these sections.
   - **Original request and intent** — Include the full request, relevant follow-up constraints, and the user's decisions with the facts each was based on, including accepted edge cases.
   - **Changes and files** — Summarize what changed and identify relevant additions, edits, deletions, and intentionally untouched areas. Provide the diff or artifact locations. Identify the exact state under review, such as a commit or file checksums, and any changes in the same workspace that belong to other work.
   - **Impact** — Describe expected behavior and relevant UX, contracts, dependencies, data, migrations, performance, security, deployment, and compatibility.
   - **Verification** — Include commands, results, failed checks, and checks that were skipped or unavailable.
   - **Caveats and earlier decisions** — Disclose assumptions and uncertainty. Include all earlier `NO` findings and what changed after each pass.
   - **Operating rules** — State which checks the reviewer may run, where it may write scratch files, and what it must not do, such as reading credentials, starting heavy jobs, or taking actions with outside effects like sending messages, changing shared data, restarting services, publishing, or deploying.
3. Present facts and evidence in chronological order where it matters. Ask the reviewer to verify the packet's claims, choose its own review focus, and inspect any relevant artifacts. Keep the packet free of advocacy, suggested priorities, and conclusions about which issues are resolved. Share evidence and observable rationale. When the packet is long, save it as a file the reviewer can read, and send a short prompt that points to it.
4. Start a fresh independent reviewer through an available delegation tool or a separate agent run. Use the model and reasoning effort the user chose for reviews, and match the main agent's for any setting the user did not choose. Set them explicitly or rely on documented inheritance, and never pick a weaker model or lower effort on your own. Start each reviewer with a clean context containing the packet, relevant artifacts, and this skill; do not fork the main conversation or reuse an earlier reviewer. Include the opening standard and reviewer instructions in the packet when the reviewer cannot load the skill. Assign the reviewer role explicitly. If the current session cannot launch a reviewer with the required settings, prepare the same packet for an independent session that the user or workflow runner can start with them.
5. Wait for the decision without status requests, follow-up messages, or scope nudges. Interrupt the reviewer only on concrete evidence that it is stuck rather than thorough.
6. On `NO`, fix each valid, feasible, in-scope finding. Favor deletion, reuse, or a smaller direct change. Record the reason for deferring an out-of-scope or risky finding, or for disagreeing with a finding contradicted by evidence or the request.
7. Treat a finding as evidence for the user, not as permission to expand the work. If handling a reviewer-found edge case would need substantial machinery or a change of observable behavior beyond the user's request, get the user's decision on the outcome first. State the exact observable violation, what triggers it, its likely practical significance, and the cost of preventing it. Offer the choice between guaranteeing the corrected outcome and accepting the edge case. Ask about outcomes, not internal mechanisms. Then implement the chosen outcome in the smallest way, and return to the user if that creates another observable tradeoff. Raise such decisions as soon as they appear.
8. Before the next pass, reread each fix in context, rerun the checks, and confirm every claim in the updated packet. Send it to a new reviewer under the same conditions.
9. Repeat until the latest reviewer issues an unequivocal `YES` or a concrete blocker prevents progress. Valid blockers include inability to run a fresh reviewer session with the required settings, inaccessible evidence needed for a decision, and a required decision beyond the granted scope. Reserve `YES` for the independent reviewer. Each pass gets a fresh session, and the loop has no fixed round limit.
10. Do not commit or push to a shared branch, merge, publish, or deploy until the latest reviewer returns `YES`. Ship only the exact reviewed state; any later edit needs another pass.
11. Report the latest decision and the most important evidence or issues at handoff. If blocked, state what prevents progress and give the latest result if one exists. Gate a successful handoff on `YES`.

## Reviewer

Keep all review work in the current reviewer session. Verify the packet's claims against the original request, the artifacts, and the available checks. Choose the review focus independently. Treat the user's recorded decisions, including accepted edge cases, as part of the request, and reopen one only with evidence the user did not have. Follow the packet's operating rules, leave the work under review unchanged, and leave scope decisions and requests for the user's approval to the main agent.

- Evaluate product behavior and engineering execution, including relevant edge cases, failure modes, races, performance, UX, accessibility, compatibility, maintainability, operational risk, and documentation.
- Identify bugs, unclear behavior, risks, missed requirements, and even small improvements in naming, clarity, or polish. Give concrete references and recommendations when possible.
- Report every valid finding, even one that is rare, hypothetical, costly to fix, or likely to need the user's decision. For each material finding, separate observed evidence from a hypothetical trigger, and state the triggering conditions, the observable consequence, its likely practical significance, and the supporting evidence.
- Prefer removing unnecessary work, deleting code, using an existing helper or dependency, relying on the standard library or native behavior, or making a smaller direct change. For each proposed addition, explain why simpler options fall short. When a larger mechanism is genuinely the best fix, recommend it and name the smaller or no-change alternatives with their tradeoffs.
- Flag speculative abstractions, unused flexibility, unrequested configuration, wrappers around one caller, and avoidable dependencies. Keep findings tied to the request and respect trust boundaries, data-loss prevention, security, and accessibility.
- Flag verification gaps that affect confidence. Ask for the smallest runnable check that could catch the relevant regression.
- Tag simplification findings with `delete`, `stdlib`, `native`, `existing`, `yagni`, or `shrink` when the tag makes the proposed action clearer.

Return only the decision, observable rationale, and evidence in this format.

```text
Decision: YES or NO
Explanation: <why no changes are needed, or every issue you would fix with concrete references>
Evidence reviewed: <files, diffs, commands, screenshots, or other artifacts inspected>
```
