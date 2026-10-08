---
title: "Day 11: Imports, matching, and reconciliation"
parent: Lessons
nav_order: 11
---

# Day 11: Imports, matching, and reconciliation

“There is already a Projects account called Maya Rao. Should we link it or create another?”

The name is a starting point, not a decision. Linking the wrong account could direct later updates or access decisions at someone else's record. Creating another account without checking could leave one person with two unmanaged paths.

Alex first establishes which person the existing account represents, which integration discovered it, and which association already exists.

By the end, explain when an existing account can be linked with confidence and when incomplete or conflicting evidence leaves the decision open.

## Separate the stages

An **import** reads records or changes from a connected system into Okta for processing. **Discovery** means the record was found within that operation's scope. **Matching** compares it with existing Okta identities. **Confirmation** accepts the proposed match or new-user outcome through the configured process.

| Stage | Question it answers | What it does not establish |
|---|---|---|
| Discovery | Which records did this operation find? | That every possible account was searched. |
| Matching | Which existing identity meets the configured comparison? | That the input data is correct. |
| Confirmation | Was this match or new-user outcome accepted? | That all subsequent operations succeeded. |
| Association | Which external record is linked to which Okta identity? | That the external system controls the profile. |
| Activation | Is the relevant account in an enabled state? | That sign-in and every required permission work. |

These are logical distinctions. Configuration may automate some stages; the administrator does not necessarily approve every record manually. Okta documents matching and confirmation options separately in its [AD import settings](https://help.okta.com/oie/en-us/content/topics/directory/ad-agent-configure-import.htm).

“Import succeeded” needs a more precise follow-up: which records were scanned, what was proposed, what was confirmed, and what happened afterward?

## Start with scope and direction

An import from Projects into Okta is different from Day 10's create request from Okta to Projects. Reading an account does not create it again.

For this lesson, the fictional Projects integration supports inbound user discovery, and its mapped employeeNumber is available for comparison. Projects remains a downstream account system, not a configured profile source. Reading its records does not change Northbridge's Workday-led architecture.

For AD, selected organizational units help define what is eligible for import. The correct domain, integration instance, scope, and read permissions matter. A record outside scope can be absent from an import without being absent from AD.

Do not widen scope just to make a missing record appear. First compare its location with the approved population. Priya's contractor identity and Alex's administrative identity are intentionally outside employee source imports.

## A matching rule is a comparison, not a biography

An **exact match** means a record meets the configured exact-match criteria. It does not mean the system independently proved the person's real-world identity. A **partial match** is a candidate meeting the configured weaker comparison, such as matching names where identifiers differ.

Okta supports [matching on mapped attributes](https://help.okta.com/oie/en-us/content/topics/users-groups-profiles/usgp-matching-imported-users.htm). The selected criteria and confirmation behavior must be inspected for the specific integration. A business review rule is not automatically a built-in Okta feature.

Northbridge uses this review policy for the following Projects packet:

- Compare the mapped employeeNumber with the existing Okta employeeNumber for this employee population.
- Accept the proposed link only when the value is present, unique among the checked candidates, and supported by the authoritative employee record and account-ownership evidence.
- Hold missing, conflicting, or ambiguous evidence for review. Do not approve a name-only candidate or create a second Okta identity as a shortcut.

The integration's configured exact comparison is employeeNumber. Manual confirmation is used for this packet. The additional ownership checks are Northbridge's review process. Its employee numbers are governed identifiers, not a claim that every organization's employeeNumber is unique, immutable, or never reused.

Email can change or be reused. Names can be shared. Even an employee number can be copied incorrectly. A reliable matching decision depends on both the comparison and the quality of the values compared.

## Work through Maya's existing account

Packet M-1 is an independent account-review scenario. It does not claim to resolve Day 10's conflict history. Maya remains a Sales employee with existing Workday and AD associations and an approved Projects assignment. Her Projects association is unconfirmed in this packet.

| Evidence | Observation |
|---|---|
| Authoritative employee record | Maya Rao, NB-1042, Sales. |
| Existing Okta identity | Maya Rao, employeeNumber NB-1042. |
| Projects candidate A | id prj-1042; name Maya Rao; employeeNumber NB-1042. |
| Projects candidate B | id prj-7788; name Maya Rao; employeeNumber NB-7788. |
| Ownership evidence | Projects owner confirms A belongs to NB-1042 and B belongs to a different person. |
| Comparison coverage | Relevant candidates and identifier uniqueness checked in the stated Projects instance. |
| Proposed match | A to Maya's existing Okta identity. |
| Confirmation | Pending. |

Candidate A meets the comparison and the supplied ownership checks. Candidate B's similar name does not override its different employee number and confirmed different ownership. The evidence supports confirming A through the approved process, not creating another Okta identity or deleting B.

Now use an explicit follow-up M-2: confirmation succeeds, and an association check links Maya's existing Okta identity to prj-1042 in the intended Projects integration. A read of that target confirms NB-1042, SAL, and active true. No sign-in is supplied.

This establishes the association and checked account state. It does not establish Maya's Projects entry. Nor does it make Projects her profile source: that integration is not configured as one. Workday still controls her employee profile under the existing source model.

## Recognize when the evidence is insufficient

Replace M-1's ownership evidence with a different packet M-3: both Projects candidates contain NB-1042, and neither has verified ownership. The original comparison's uniqueness condition is no longer met.

Do not choose the first result. Request account-creation records, the accountable application owner's evidence, and the authoritative identifier history. If a value was copied incorrectly, correct it through the responsible process before rerunning the comparison. If one person has duplicate accounts, the business owner must decide their intended use and consolidation; a duplicate-looking name alone does not authorize deletion.

A missing employeeNumber is also unresolved. It is not evidence that the person is new. Different issues can produce “no match”: a new employee, a missing value, a transformation error, the wrong scope, or incomplete discovery. Distinguish these before creating an identity.

## Association can affect sourcing

An application-account association and an applicable profile-source association are related concepts, but not interchangeable. The integration's source settings matter.

Recall Northbridge's order: Workday above AD. Maya already has both associations; matching a Projects account does not move Projects into that order. Department inherits Workday ownership, while her designated AD-owned email remains a field-level exception.

Consider a separate hypothetical error: an AD record is accidentally confirmed against Priya's Okta-managed contractor identity. This violates her intended exclusion from employee imports. Because AD is a configured profile source, the new association may affect her source applicability and governed values. Inspect the resulting source, attribute settings, and actual writes. Do not assume the contractor label prevents the effect or that Workday applies to her without an association.

The correction requires investigating the mistaken match and its impact, then restoring the intended association and data through the supported process. Merely changing source priority for everyone would address a much broader decision. See the [ownership reference](../reference/attribute-ownership.md) and [Okta's source-priority documentation](https://help.okta.com/oie/en-us/content/topics/users-groups-profiles/usgp-prioritize-profile-source.htm).

## Reconcile expected and observed records

**Reconciliation** means comparing the intended records and relationships with the observed ones and explaining differences. Here it is an investigation practice, not the name of a promised automatic repair feature.

| Expected | Observed | Useful next question |
|---|---|---|
| One correctly associated account | No confirmed association | Was it discovered, matched, and confirmed? |
| One account for the approved purpose | Two plausible candidates | Who owns each, and are both authorized? |
| An employee is in import scope | No imported record | Was the correct scope searched successfully? |
| Priya remains Okta-managed | Unexpected AD association | Which match created the link, and what changed afterward? |

Count differences help locate a problem but do not identify individual ownership. “100 imported” does not prove the 100 expected people were correctly matched.

For example, 99 expected employees plus one unexpected account still total 100. Compare the actual identities and associations, not just the totals.

## Read the whole result, not just its first page

Alex receives a synthetic read-only Okta management API result for `GET /api/v1/users?limit=2`. This is an evidence excerpt, not a request to run. The fictional host represents the checked Okta organization.

```text
HTTP/1.1 200 OK
Link: <https://identity.northbridge.example/api/v1/users?limit=2&after=CURSOR_PLACEHOLDER>; rel="next"
```

Selected fields from that page's two user objects are:

```json
[
  {"id": "OKTA_MAYA_PLACEHOLDER", "profile": {"login": "maya.rao@northbridge.example"}},
  {"id": "OKTA_DANIEL_PLACEHOLDER", "profile": {"login": "daniel.brooks@northbridge.example"}}
]
```

Priya is not shown. That does not establish her absence: the next link explicitly indicates another page. A **cursor** marks a position in the result sequence. It is **opaque**, meaning the caller uses the service's value without interpreting or constructing it. Follow the supplied next link rather than inventing a page number. Check query and endpoint scope as well as pagination before making a completeness claim. See [Okta API pagination](https://developer.okta.com/docs/reference/core-okta-api/#pagination).

This reads Okta users; it is not Projects' SCIM list response and does not enumerate Projects accounts. This JSON excerpt does not supply association evidence.

In a separate attempt, the same read receives HTTP 403 Forbidden, with an investigator's supplied finding that the caller lacks the required permission. That is a failed read, not an empty result. Request an appropriately authorized read through the responsible administrator; do not infer “no users” or treat the failed request as a reason to create a duplicate. A bare 403 without that finding would require more investigation of the rejection.

## Compare import with just-in-time creation

An import discovers accounts through a read operation. **Just-in-time provisioning (JIT)** creates or updates an account during a supported, configured sign-in journey. Always name the destination: an application creating its own account from federation is different from Okta creating a profile through an enabled AD JIT path.

Successful federation alone does not guarantee either behavior. Northbridge's Projects matching packet uses import and confirmation; it does not assume JIT. JIT also does not eliminate the need for identity matching, ownership, and later access-removal decisions.

## Treat an unexpected removal count as evidence

An import safeguard can stop covered imports when proposed unassignments reach a configured threshold. Its scope and eligibility matter: it is not a universal barrier against every access change. In particular, a changed attribute affecting a group rule is not the same as a covered imported-group membership removal. See [import safeguard coverage](https://help.okta.com/oie/en-us/content/topics/users-groups-profiles/usgp-import-safeguard.htm).

Suppose an import reports an unexpected rise in removals after its selected directory scope changes. Compare the intended population, the changed scope, source availability, and affected records before treating those removals as departures. Ask the source owner to confirm the business changes. Do not disable the safeguard merely to finish the import.

Once the cause is established, review the corrected import results and the remaining associations and assignments. A count returning to normal does not prove that the right identities were preserved. For an unfamiliar matching and assignment situation, try [the missing-application request](../requests/missing-application.md) after the foundation checkpoint.

## Before moving on

Can you explain why a discovered record is not yet a verified association, why an exact comparison can use bad data, and why M-3 must remain unresolved? Can you distinguish an app association from profile ownership and an incomplete read from confirmed absence?

Try the [exercises](../exercises/day-11.md), then the [answers](../self-checks/day-11.md). In [your notebook](../notebook/guide.md), record the integration, source record ID, intended Okta identity, matching criteria, ownership evidence, confirmation result, and remaining unknowns.

[Day 12](day-12-joiners-movers-leavers.md) follows an approved move and departure through these connected records.

[Previous: Day 10](day-10-scim.md) · [Course home](../index.md)

[Revise Day 11](../revision/day-11.md): revisit the concepts, example, and questions without rereading the full lesson.
