# Integration checkpoint: Debrief and rubric

Attempt the [checkpoint](../integration.md) first. Equivalent reasoning is acceptable; judge whether the conclusion follows from the packet.

## Task 1: Salesforce

This is SAML sign-in. Response issuance succeeded on Okta's side; Salesforce rejected the supplied test audience against its production expectation. Signature acceptance is separately supplied and does not make the audience acceptable. Daniel's account exists and is active, but this attempt did not establish a session.

Review the production connection's emitted audience configuration against its approved identifier, correct the demonstrated mismatch, and verify a new response and Salesforce acceptance. Keep account matching, session establishment, and required permissions as distinct checks. No evidence supports an authenticator reset or a new account as the correction.

## Task 2: Expense

This is OIDC sign-in through Authorization Code with PKCE. The browser returns a code; the backend exchanges it for tokens. Decoded claims provide readable values, while the supplied validation report establishes the stated checks for this attempt.

The intended account already exists, and actual entry and an application session are demonstrated. Creation is not: the packet explicitly describes a pre-existing account. Expense-approval permission remains untested.

## Task 3: Projects

This is a SCIM account-create exchange. The request reached Projects and received a reported uniqueness conflict. No successful create is established. Projects reports a value already in use, but the packet does not identify its owner or establish a correctly associated Maya account.

Request the conflicting resource and reliable ownership identifiers, the matching/lookup evidence, and the correlated operation history. Use those to decide approved existing-account handling or a data correction. Do not automatically delete, rename, or link a record. Successful Expense sign-in concerns a different application and operation.

## Task 4: Proposed shortcut

The Expense ID token supplies authentication information for Expense's client validation. It is not a service credential granting the Projects connector account-management authority. Projects' uniqueness constraint also protects its account model; bypassing it would not resolve the identity question.

The supplied create reached the target through an authorized connector. The next justified action is the ownership/matching investigation from Task 3, not substituting credentials or disabling validation.

## Task 5: Handoff

| Application | Supported outcome | Next action and resolution evidence |
|---|---|---|
| Salesforce | Daniel's account exists; A-1 failed audience validation and did not establish a session. | Correct the emitted audience for the approved production connection, then verify a new correlated exchange and application entry. |
| Expense | Maya's pre-existing account and B-1 entry are confirmed. | No sign-in defect is shown. Check approval permission separately if the business requirement includes approving expenses. |
| Projects | Create rejected; ownership and correct association of the conflicting record are unknown. | Establish record ownership and lookup history, choose the approved correction, then verify association, applicable operation result, and target state. Entry requires separate evidence. |

A good handoff distinguishes observed facts from proposed actions. “All provisioning works because SSO succeeded” would cross both a protocol boundary and an application boundary.

## Assess your reasoning

For each criterion, mark **independent**, **with guidance**, or **needs review**. Independent means you explained it before the debrief; with guidance means a reference helped; needs review means the distinction remains unclear.

| Criterion | Evidence of understanding | Revisit |
|---|---|---|
| Protocol purpose | Separates SAML/OIDC sign-in from SCIM account management. | Days 8–10 |
| Message route | Separates browser delivery from backend exchanges. | Days 2 and 9 |
| Validation | Does not equate readable claims or issuance with target acceptance. | Days 8–9 |
| Account evidence | Distinguishes a confirmed account from an unowned conflict report. | Days 1 and 10 |
| Correction | Fixes the audience defect but investigates ownership before resolving the conflict. | Day 10 conflict example |
| Scope | Keeps people, instances, attempts, permissions, and outcomes separate. | Day 2 evidence boundaries |

## Try a changed condition

Replace Packet C's conflict with a 201 response containing the new resource id and a subsequent target read confirming Maya, SAL, and active true. No Projects sign-in is supplied. What changes?

<details>
<summary>Check the changed-condition reasoning</summary>

Creation and the checked account state are now supported, so the uniqueness investigation no longer follows from this packet. Projects sign-in and project permissions remain unverified. Expense success still cannot establish those outcomes for Projects.

</details>

[Checkpoint](../integration.md) · [Day 10](../../lessons/day-10-scim.md) · [Course home](../../README.md)
