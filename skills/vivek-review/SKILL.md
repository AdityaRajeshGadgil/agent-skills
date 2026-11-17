---
name: vivek-review
description: Run an independent ship-readiness review of completed changes to code, products, documents, configuration, tests, deployments, or generated artifacts. Use before final handoff, committing to a shared branch, any Git push of the change, opening or merging a pull request, publication, or deployment. Also use when asked for Vivek Review, a ship/no-ship decision, a product and engineering review, or a simplicity pass. Use the user's chosen review model and reasoning effort, inheriting any unspecified setting from the main agent.
---

# Vivek Review

Ask whether, applying the highest standards of product judgment and engineering, you would be proud to ship the completed change exactly as it stands. Return `YES` only if you would change nothing. If you would fix even a small issue, return `NO` and report all your findings.

## Main agent

1. Finish the change you intend to ship and run the applicable checks.
2. Assemble a neutral review packet with these sections.
   - **Original request and intent** — Include the full request, relevant follow-up constraints, and the user's decisions with the facts each was based on, including accepted edge cases.
   - **Changes and files** — Summarize relevant edits, additions, deletions, and intentionally untouched areas. Supply paths the reviewer can read. If it cannot access the files or check results, attach or inline the relevant evidence it is permitted to see. Identify the exact state under review, such as a commit or file checksums, and any changes in the same workspace that belong to other work.
   - **Impact** — Describe expected behavior and relevant UX, contracts, dependencies, data, migrations, performance, security, deployment, and compatibility.
   - **Verification** — Include commands, results, failed checks, and checks that were skipped or unavailable.
   - **Caveats and earlier decisions** — Disclose assumptions and uncertainty. Include all earlier `NO` findings and what changed after each pass.
   - **Operating rules** — State which checks the reviewer may run, where it may write scratch files, and what it must not do, such as reading credentials, starting heavy jobs, or taking actions with outside effects like sending messages, changing shared data, restarting services, publishing, or deploying.
3. Present facts and evidence in chronological order where it matters. Ask the reviewer to verify the packet's claims, choose its own review focus, and inspect any relevant artifacts. Keep the packet free of advocacy, suggested priorities, and conclusions about which issues are resolved. For a long packet, send a short file pointer if the reviewer can open it. Otherwise deliver the full packet in the prompt or a readable attachment.
4. Start a fresh independent reviewer through native delegation or a separate runner. Use the user's chosen review model and reasoning effort. For each setting the user left open, match the main agent through explicit settings or documented inheritance. Assign the reviewer role and give it only this skill, the packet, and relevant artifacts. If it cannot load the skill, include the opening standard and reviewer instructions in the packet.

   Use a new session for each pass. A conversation fork or a resumed reviewer carries prior context. If a new or changed launcher has uncertain defaults, use a harmless probe to confirm the reviewer receives only the permitted context and tools. If a launcher is missing, check tool discovery when available. If this session still cannot start a qualifying reviewer, prepare the packet for an independent session that the user or workflow runner can start.
5. Let the reviewer finish without imposing a deadline, sending status requests, or steering its scope. If a wait call times out while the reviewer is still running, keep waiting. Intervene only on concrete evidence that it cannot progress. If the run ends before the reviewer delivers its final decision, explanation, and evidence, discard partial output, record what happened, and retry with a fresh reviewer when feasible.
6. On `NO`, look for other occurrences of each confirmed defect within the affected change. Fix all valid, feasible, in-scope findings in the same pass. Favor deletion, reuse, or a smaller direct change. Record the reason for deferring an out-of-scope or risky finding, or for disagreeing with a finding contradicted by evidence or the request.
7. Treat a finding as evidence for the user, not as permission to expand the work. If handling a reviewer-found edge case would need substantial machinery or a change of observable behavior beyond the user's request, get the user's decision on the outcome first. State the exact observable violation, what triggers it, its likely practical significance, and the cost of preventing it. Offer the choice between guaranteeing the corrected outcome and accepting the edge case. Ask about outcomes, not internal mechanisms. Then implement the chosen outcome in the smallest way, and return to the user if that creates another observable tradeoff. Raise such decisions as soon as they appear.
8. Before the next pass, reread each fix in context, rerun the checks, and confirm every claim in the updated packet. Send it to a new reviewer under the same conditions.
9. Repeat without a fixed round limit until the independent reviewer issues an unequivocal `YES` or a concrete blocker prevents progress. Blockers include an unavailable reviewer with the required settings, inaccessible evidence needed for a decision, or a scope decision only the user can make.
10. Obtain `YES` before committing to a shared branch, pushing the change to any remote, opening or merging a pull request, publishing, or deploying. Ship only the reviewed state. If it changes, or you discover a material error or omission in the packet, update the packet and obtain a fresh decision. Carry out only the release actions the user authorized.
11. Report the latest decision and the most important evidence or issues at handoff. If blocked, state what prevents progress and give the latest result if one exists. Gate a successful handoff on `YES`.

## Reviewer

Keep all review work in the current reviewer session. Verify the packet's claims against the original request, the artifacts, and the available checks. Choose the review focus independently. Treat the user's recorded decisions, including accepted edge cases, as part of the request, and reopen one only with evidence the user did not have. Follow the packet's operating rules, leave the work under review unchanged, and leave scope decisions and requests for the user's approval to the main agent.

- Evaluate product behavior and engineering execution, including relevant edge cases, failure modes, races, performance, UX, accessibility, compatibility, maintainability, operational risk, and documentation.
- Identify bugs, unclear behavior, risks, missed requirements, and even small improvements in naming, clarity, or polish. When the same defect recurs within the change, report the affected locations together. Give concrete references and recommendations when possible.
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
