# Owner workflow — 3 PASS + parallel Muse review + reset-on-confirmed-bug

> **Owner directive:** 2026-11-10 (Asia/Bangkok)  
> **Status:** Owner-agreed workflow, documented for AI agents; file proposed via PR pending merge.  
> **Scope:** AI Collaboration Hub and pilot project `dvphuong20786/video-ai`; applies to newly authorized implementation tasks.  
> **Participants:** Owner (human), ChatGPT (developer/tester when assigned), Muse AI (parallel independent reviewer/tester), other AI reviewers/QA.

## 0. New Owner amendment — bug reports from ANY AI or person, EVEN AFTER 3/3 PASS

**Three PASS results never immunize a task against a later valid bug report.** Muse, Jules/Gemini, other AIs, the Owner or any human tester may report a finding **at any point, including after PASS 3, reviewer approval, merge or task closure**.

Every report must be acknowledged, investigated on the reported/current SHA and **technically challenged or confirmed using reproducible evidence** (reproduction steps, file/line, logs/tests). A report is not automatically true just because it comes from a reviewer; likewise 3/3 PASS is not grounds to dismiss it.

- **Confirmed before merge:** mark previous PASS approval invalid, reopen the implementation task, return to **task intake/assignment → analysis → fix → fresh PASS 1 with immediate GitHub notification → regression PASS 2 → regression PASS 3 → second GitHub notification → Muse/AI review → approval/merge**. Same Issue/PR may be reused, with an explicit new test-cycle ID and current SHA.
- **Confirmed after merge or DONE:** reopen the task **or create a linked fix Issue**, obtain/record assignment and priority, branch from the appropriate current base, investigate/fix and run the **full workflow from task intake again**. Do **not** rewrite history or pretend earlier PASS results never happened; label them as valid only for the old SHA but **superseded by the new finding**. Use normal approval/merge policies.
- **Disproved finding:** reply publicly on the Issue/PR with reproducible counter-evidence, request confirmation from the reporter, keep the current status unless evidence changes.
- **Still disputed:** mark `DISPUTED` / `NEEDS_VERIFICATION` / `NEEDS_OWNER`; do not claim the reported issue is definitively fixed. Escalate rather than looping without proof.

This applies equally whether the reporter is Muse, any other AI, or any person.

## 1. Core rule

**A task is technically eligible for acceptance only when its *current code revision* has 3/3 real PASS results:**

1. **PASS 1 — Developer verification:** build/lint/unit and relevant feature tests; actual execution and evidence required.
2. **PASS 2 — Regression round 1:** integration, affected user flows, existing behavior and representative edge cases; actual execution required.
3. **PASS 3 — Regression round 2:** independently repeat critical/affected regression scenarios (different cases/data where useful) and verify stability; actual execution required.

PASS 2 and PASS 3 are **two additional regression test rounds**, not two status messages about one run. Muse's tests run **in parallel** and provide independent challenge; they do not automatically replace ChatGPT's PASS 1–3.

## 2. Event sequence — mandatory GitHub communication

1. **Assignment:** Muse/Owner creates or designates GitHub Issue with scope, acceptance criteria, owner/assignee and target base SHA. Developer acknowledges; only start code work with current authorization.
2. **Code:** Developer works on its own branch and PR (prefer Draft until ready); do not modify other AI branches or merge `main` without applicable approval.
3. **PASS 1:** Run verification against a **specific commit SHA**. **Immediately post to the task Issue/PR and notify Muse**: status `PASS 1/3`, SHA, commands/cases, actual result, evidence links, uncovered risks. Do **not** wait for PASS 2 or PASS 3 to report.
4. **Parallel work:** **Immediately proceed** to regression rounds **PASS 2** and **PASS 3** without waiting for Muse. At the same time, Muse may inspect and test the developer's branch/PR at any time; exchange findings on GitHub.
5. **Final regression update:** If rounds 2 and 3 both pass, **post a second GitHub report** with `PASS 3/3`, links to evidence for **all three passes**, current SHA, remaining open findings and request for Muse technical approval. This is **not** permission to auto-merge.
6. **Any AI/person challenge at any moment (including after 3/3 PASS):** If Muse, Jules, another AI or any human reports a bug, developer **reproduces/validates independently** and may debate with file/line/SHA/log/test evidence:
   - **Confirmed bug:** return to **task intake/assignment and fix analysis**, mark test cycle invalid for acceptance, fix, update SHA, notify all reviewers and **restart full 3-PASS workflow from PASS 1**; report PASS 1 immediately and run both regression rounds again. For post-merge bugs create/reopen a linked fix task as described in §0.
   - **Disproved/false positive:** respond with reproducible counter-evidence and continue the current test cycle **if the code has not changed**. Do not mark the review resolved unilaterally without agreement.
   - **Unresolved dispute or unavailable test environment:** explicitly mark `NEEDS_VERIFICATION` / `NEEDS_OWNER`; do not claim 3/3 PASS or success without proof.
7. **After 3/3 PASS + resolution:** Muse/authorized reviewer approves the final SHA, all blocking findings addressed, CI/branch and merge requirements satisfied. Owner's merge policy remains in force.
8. **Merge and post-merge:** Authorized actor merges; verify resulting `main` by available build/smoke tests, update/close Issue, publish user-facing summary (what changed, tests, Muse response, decision, links).

## 3. PASS validity and concurrency guardrails

- Every PASS report includes **repo, Issue/PR link, branch, commit SHA, tested environment, command/case list, evidence, pass/fail, and unresolved concerns**.
- **Never fabricate PASS** from static code review, proposed tests, simulated logs, or "expected success". If a test cannot be executed, label it `NOT_RUN` or `BLOCKED`.
- PASS 1 / PASS 2 / PASS 3 and Muse review must reference **the same final tested revision**; if implementation changes after a PASS, repeat affected verification and, after confirmed bug fix, **restart all 3 PASS**.
- Muse should verify the **latest branch HEAD** before asserting a finding; stale-SHA findings need reconfirmation, not automatic rejection.
- Findings from **any AI or human** may arrive **during PASS 2/3, after 3/3 PASS, after approval, after merge or after DONE**. Confirmed findings reopen the entire task/fix intake and 3-PASS cycle; before merge they block approval, after merge they require a linked fix task and ordinary release controls.
- No agent treats silence as approval; technical test success, reviewer approval, merge, and post-merge completion are **distinct statuses**.

## 4. Standard status labels for GitHub comments

- `ASSIGNED` → `IN_PROGRESS` → `PASS_1_REPORTED` → `REGRESSION_2` → `REGRESSION_3` → `PASS_3_REPORTED` → `REVIEW_APPROVED` → `MERGED` → `POST_MERGE_VERIFIED` → `DONE`
- On confirmed bug (from any source, including after 3/3 PASS): `BUG_REPORTED` → `VERIFYING` → `BUG_CONFIRMED` → `TASK_REOPENED` / `FIX_TASK_CREATED` → `ASSIGNED` → `IN_PROGRESS` → **return to `PASS_1_REPORTED` only after a fresh, evidenced PASS 1**, then PASS 2 and PASS 3.
- On disagreement: `DISPUTED` / `NEEDS_OWNER`; on inability to run tests: `BLOCKED` / `NOT_RUN`.

### PASS 1 notification example

```md
[ChatGPT → Muse] PASS 1/3 — Issue #..., PR #...
Branch: ... | Commit: ...
Executed: ... | Result: PASS | Evidence: ...
Next: Continue regression PASS 2 and PASS 3 now; Muse may test/review the same SHA in parallel.
```

### Final regression notification example

```md
[ChatGPT → Muse] PASS 3/3 — Issue #..., PR #...
Branch: ... | Final commit: ...
PASS 1: ... (evidence)
PASS 2: ... (evidence)
PASS 3: ... (evidence)
Muse findings: [resolved / awaiting ...]
Status: Ready for independent approval; NOT merged.
```

### Confirmed finding from Muse / other AI / any human example

```md
[ChatGPT → reporter, Muse, Owner] BUG CONFIRMED — reopen task and restart entire workflow
Finding: ... | Reproduction: ... | Prior SHA: ... | Fix SHA: ...
Previous 3/3 PASS is superseded for this issue/SHA. Next: task re-acceptance, analysis, fix, fresh PASS 1 report, regression rounds 2 and 3, new final report.
```

## 5. Boundaries / current ownership

- Owner approval remains required for changes/merges where project policy says so; this document alone does **not** authorize coding or merging.
- On 2026-11-10 Muse explicitly **withdrew M1–M7 implementation assignments from ChatGPT** and assigned Muse to code them; **ChatGPT remains review-only until Owner explicitly changes that decision**. This workflow is for future authorized task assignments, not a reinstatement of the withdrawn tasks.
- Do not initiate separate paid AI API calls without Owner approval. Respect any current review scope exclusions separately from this workflow.

## 6. Source threads / request for acknowledgment

- Coordination and decision thread: https://github.com/dvphuong20786/AI-Collaboration-Hub/issues/1
- Video AI ongoing technical review: https://github.com/dvphuong20786/video-ai/issues/14
- Muse, Jules/Gemini and other agents: please reply on the Hub issue with **ACK / proposed amendment / evidence-backed objection**, then apply these stage gates to your own work once the owner-approved policy is published and the task is assigned.
