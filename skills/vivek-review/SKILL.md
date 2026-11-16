---
name: vivek-review
description: Run a strict independent ship-readiness review after development work on code, products, documents, configuration, tests, deployments, or generated artifacts. Use before final handoff and when asked for Vivek Review, a ship/no-ship decision, an engineering and product quality review, or a simplicity pass. Send a neutral evidence packet to a fresh reviewer session with the same model and reasoning effort, then address findings and repeat until an unequivocal YES or a concrete blocker.
---

# Vivek Review

Ask whether, applying the highest standards of product judgment and engineering, you would be proud to ship the completed change exactly as it stands. Return `YES` only if you would change nothing. Return `NO` for every issue you would fix, including small polish issues.

## Main agent

1. Finish the requested work and run the applicable checks.
2. Assemble a neutral review packet with these sections.
   - **Original request and intent** — Include the full request and relevant follow-up constraints.
   - **Changes and files** — Summarize what changed and identify relevant additions, edits, deletions, and intentionally untouched areas. Provide the diff or artifact locations.
   - **Impact** — Describe expected behavior and relevant UX, contracts, dependencies, data, migrations, performance, security, deployment, and compatibility.
   - **Verification** — Include commands, results, failed checks, and checks that were skipped or unavailable.
   - **Caveats and earlier decisions** — Disclose assumptions and uncertainty. Include all earlier `NO` findings and what changed after each pass.
3. Present facts and evidence in chronological order where it matters. Ask the reviewer to verify the packet's claims, choose their own review focus, and inspect any relevant artifacts. Keep the packet free of advocacy, suggested priorities, and conclusions about which issues are resolved. Share evidence and observable rationale.
4. Start a fresh independent reviewer through an available delegation tool or a separate agent run. Match the main agent's model and reasoning effort using explicit settings or documented inheritance. Start with a clean context containing the packet, relevant artifacts, and this skill. Include the opening standard and reviewer instructions in the packet when the reviewer cannot load the skill. Assign the reviewer role explicitly. If the current session cannot launch a reviewer, prepare the same packet for an independent session that the user or workflow runner can start with matching settings.
5. Wait for the decision. On `NO`, fix each valid, feasible, in-scope finding. Record the reason for deferring an out-of-scope or risky finding, or for disagreeing with a finding contradicted by evidence or the request. Favor deletion, reuse, or a smaller direct change. Update the packet and send it to a new reviewer under the same conditions.
6. Repeat until the latest reviewer issues an unequivocal `YES` or a concrete blocker prevents progress. Valid blockers include inability to run a fresh reviewer session with matching settings, inaccessible evidence needed for a decision, and a required decision beyond the granted scope. Reserve `YES` for the independent reviewer. Each pass gets a fresh session, and the loop has no fixed round limit.
7. Report the latest decision and the most important evidence or issues at handoff. If blocked, state what prevents progress and give the latest result if one exists. Gate a successful handoff on `YES`.

## Reviewer

Keep all review work in the current reviewer session. Verify the packet's claims against the original request, the artifacts, and the available checks. Choose the review focus independently.

- Evaluate product behavior and engineering execution, including relevant edge cases, UX, accessibility, compatibility, maintainability, and documentation.
- Identify bugs, unclear behavior, risks, missed requirements, and even small improvements in naming, clarity, or polish. Give concrete references and recommendations when possible.
- Prefer removing unnecessary work, deleting code, using an existing helper or dependency, relying on the standard library or native behavior, or making a smaller direct change. For each proposed addition, explain why simpler options fall short.
- Flag speculative abstractions, unused flexibility, unrequested configuration, wrappers around one caller, and avoidable dependencies. Keep findings tied to the request and respect trust boundaries, data-loss prevention, security, and accessibility.
- Flag verification gaps that affect confidence. Ask for the smallest runnable check that could catch the relevant regression.
- Tag simplification findings with `delete`, `stdlib`, `native`, `existing`, `yagni`, or `shrink` when the tag makes the proposed action clearer.

Return only the decision, observable rationale, and evidence in this format.

```text
Decision: YES or NO
Explanation: <why no changes are needed, or every issue you would fix with concrete references>
Evidence reviewed: <files, diffs, commands, screenshots, or other artifacts inspected>
```
