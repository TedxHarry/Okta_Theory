---
title: "Notebook"
nav_order: 8
---

# Your learning notebook

Keep a short record of explanations you can use again. Write in your own words; copying a definition is less useful than explaining what happens to someone at Northbridge.

For each lesson, record:

- A flow you can explain from beginning to end.
- Two concepts that are easy to confuse and the difference between them.
- One observation and what it proves.
- One question that remains unanswered and the evidence you would request.

## An investigation entry

```text
Person and application:
Expected behavior:
Observed behavior:
Evidence available:
What that evidence proves:
What it does not prove:
Possible explanations:
Next evidence to request:
How different findings would change my explanation:
Correction, if the cause is established:
How to verify the result:
```

You do not need a confirmed correction when the evidence is incomplete. Saying what remains unknown and how to investigate it is a useful answer.

## Choose evidence that separates explanations

After Day 3, consider Daniel's app user profile containing FIN while Projects still contains SAL. Two possible explanations are that no update occurred or that an attempted update was rejected.

“Check the logs” is too broad to distinguish them. Ask for the account-update history for Daniel's linked Projects account, including the value sent and the response. Evidence of a rejected FIN update supports investigating that rejection. Evidence that updates were disabled and no operation occurred points toward the configured update process. An empty search alone does not prove no update happened: check its scope.

Write what finding would make you change your mind. This turns a list of possible causes into an investigation.

## Reuse an earlier explanation

Before starting the next lesson, close the earlier lesson and explain one of its flows from your notebook. Then compare your explanation with its self-check. Keep a short correction beside any distinction you missed. As new evidence appears, update the same flow rather than starting an unrelated set of notes.

[Return to Day 1](../lessons/day-01-people-identities-access.md)

## After the final case

Use the [final skills checklist](../assessments/self-checks/final-case.md) to select one explanation to improve. Keep the original conclusion, the evidence that changed it, and the revised explanation together. This makes your reasoning visible when a later case looks similar but has different facts.

## An access-decision entry

Use this after Day 14 to explain an approved access requirement and its proposed design. Keep approval decisions separate from observations showing whether the design works.

```text
Application and instance:
Business owner and approved requirement:
Eligible people and effective condition:
Permitted application actions:
Source attributes and their owners:
Normal group and assignment path:
Exception approval, scope, owner, and end condition:
Existing-account matching and target-account handling:
Authentication requirement:
Administrative authority needed:
People and applications affected by a change:
Evidence that intended access works:
Evidence that unapproved access is denied:
Move, departure, and exception-removal evidence:
Recovery if the change produces an incorrect outcome:
Unresolved decisions and their owners:
```

For R-14, Finance employee eligibility belongs in the normal path. Daniel's separate approver approval belongs under permitted actions. Priya's missing exception decision stays unresolved; the template is not permission to fill it with a guess.

[Day 15](../lessons/day-15-integrated-case.md) · [Course home](../README.md)

