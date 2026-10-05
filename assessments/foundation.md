# Foundation checkpoint: From identity data to access

Explain Northbridge's access decisions using Days 1–5. Write your reasoning before opening the [separate debrief](self-checks/foundation.md). You may use your notebook and references; record where you needed help.

All packets are synthetic. B, C, and D are independent variations on A, not consecutive incidents. Maya remains a Sales employee except in D's explicitly hypothetical move.

## Packet A: Expected access

Northbridge assigns Salesforce automatically to Sales employees through NB-Sales-Employees. Contractors require a separately approved exception. The rule is active, both users are Active and processed, neither is excluded, and no other membership mechanism supplies this group.

| Evidence | Maya | Priya |
|---|---|---|
| Source associations | Workday and AD | Neither |
| Effective profile source | Workday; above AD in priority | Okta |
| Department and workerType sourcing | Inherit from profile source | Inherit from profile source |
| Approved classification | Sales / Employee | Sales / Contractor |
| Okta user profile | Sales / Employee | Sales / Contractor |
| Rule | Sales AND Employee → NB-Sales-Employees | Same rule |
| Observed membership | Present | Absent |
| Salesforce assignment | Through NB-Sales-Employees | None; other paths checked |
| Target-account and Salesforce sign-in evidence | Not supplied | Not supplied |

**Task 1:** Tell each person's source-to-assignment story. Explain the different result and what remains unknown beyond assignment.

## Packet B: A correct rule receives the wrong value

Replace only Maya's data and result observations with these:

| Evidence | Observation |
|---|---|
| Workday department | Sales, confirmed as the approved value |
| Checked source association and department owner | Correct Maya record; Workday controls department |
| Current incoming department mapping | Always returns Finance |
| Preview for the Sales input | Finance |
| Okta department / workerType | Finance / Employee |
| Rule processing and membership | Rule processed that profile; Maya absent |
| Salesforce assignment | Absent; other paths checked |

**Task 2:** Identify the demonstrated defect. Propose a correction consistent with the business requirement and specify how to verify it. Does the packet establish the complete history of every field write?

## Packet C: Priya has a different assignment path

Keep Priya's correct Packet A profile and nonmembership, but replace her assignment state with an individual Salesforce assignment. Its approval record and target-account evidence are not supplied.

An Okta SSO event concerning Priya and Salesforce records SUCCESS for a specified attempt. No application-side acceptance or permission evidence is included.

**Task 3:** Explain why her assignment does not demonstrate a rule failure. Identify the missing business decision and the limits of the event. Name a specific next evidence request and explain how different findings would change your conclusion.

## Packet D: Predict a future change

Starting from A, suppose Maya later has an approved move to Finance. Workday and Okta both receive Finance, and the active rule processes her updated profile. Assume this rule is the sole membership mechanism for NB-Sales-Employees and there is no individual exclusion.

**Task 4:** Predict her Sales-group membership. Contrast two assignment states: no other Salesforce path exists, or another checked group still assigns Salesforce. What target and session evidence would you need before claiming the end of all Salesforce access?

## Explain the architecture

**Task 5:** Explain why none of these are interchangeable: profile source, mapping, group rule, application assignment, provisioning result, and SSO event. Use at least one observation above for each relevant boundary. Explain your next action to a manager without relying on menu names.

Use the [debrief and rubric](self-checks/foundation.md) to assess your reasoning. Keep the corrected flow in [your notebook](../notebook/guide.md).

[Return to Day 5](../lessons/day-05-groups-and-assignments.md) · [Course home](../README.md)
