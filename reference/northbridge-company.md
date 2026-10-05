---
title: "Northbridge Services"
parent: Reference
nav_order: 1
---

# Northbridge Services

Northbridge Services has Sales, Finance, and IT teams. Employees and contractors need access to different applications. The company must also change that access when people's responsibilities change or their work ends.

## People

| Person | Role |
|---|---|
| Maya Rao | Sales employee who later moves to Finance. |
| Daniel Brooks | Finance manager. |
| Priya Shah | Sales contractor with an employee sponsor. |
| Alex Chen | IAM administrator; the administrative identity is managed in Okta. |
| Jordan Lee | Employee whose later departure requires access removal. |

## Systems

| System | Purpose |
|---|---|
| Workday | Employee records and employment information. |
| Active Directory | Directory accounts and groups; validates employee passwords when the configured AD password sign-in path is used. |
| Okta | Workforce identity records, application assignments, sign-in decisions, and configured connections to other systems. |
| Salesforce | Sales application. |
| ServiceNow | Background IT service application; no separate ServiceNow investigation is required in this course. |
| Microsoft 365 | Workplace applications. |
| Northbridge Expense | Fictional expense application used for an OIDC sign-in example. |
| Northbridge Projects | Fictional application used for account-management examples through SCIM. |
| Okta Verify | An authenticator used in sign-in examples. |

The initial picture is simple:

```text
Employee information in Workday
              ↓
         Identity in Okta
              ↓
   Configured connections to AD and applications
```

The arrows show logical relationships. They do not mean that every account exists immediately or that every application uses the same protocol.

Employee information is HR-led. Contractor identities such as Priya's are maintained in Okta following sponsor approval. The detailed source ownership and sign-in decisions become part of the examples as those concepts are introduced.

[Source and ownership reference](attribute-ownership.md) · [Return to Day 1](../lessons/day-01-people-identities-access.md) · [Course home](../README.md)

## Follow the timeline

Days 1 to 11 use Maya's Sales employment and Jordan's active employment unless a packet explicitly states a hypothetical variation. In [Day 12](../lessons/day-12-joiners-movers-leavers.md), Maya's approved move to Finance and Jordan's departure take effect. Earlier observations remain evidence about their earlier states. Individual case packets state their own account and assignment evidence.

## Keep Maya's identifiers separate

Different fields can identify the same person for different purposes. These values appear in the relevant teaching packets:

| Field and context | Value | What to compare |
|---|---|---|
| Employee number | `NB-1042` | The governed employee record and the stated matching criteria. |
| AD-owned work email in Day 4 | `mrao@northbridge.example` | The linked AD value and Okta's work-email field. |
| Okta login in Day 11's API excerpt | `maya.rao@northbridge.example` | The login field on the intended Okta user. |
| Salesforce username and SAML NameID in Day 8 | `maya.rao@northbridge.example` | The connection's configured username-matching rule. |
| Projects resource identifier | `prj-1042` | The confirmed association to the intended Projects account. |

The work-email field, Okta login, and application username are separate attributes even when some contain identical text. Day 4's email ownership does not define the login or SAML NameID mapping. No example establishes that `mrao@northbridge.example` and `maya.rao@northbridge.example` are mailbox aliases. Read the named field and its configured relationship before treating different strings as an error.

