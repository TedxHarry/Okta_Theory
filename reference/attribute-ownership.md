---
title: "Northbridge's source and ownership reference"
parent: Reference
nav_order: 2
---

# Northbridge's source and ownership reference

Use this alongside [Day 4](../lessons/day-04-sources-and-ownership.md). These are Northbridge's stated architecture decisions, not universal settings for every organization.

The employee flow is Workday → Okta → AD and applications. AD can also return designated directory-owned values. External profile sources are ordered Workday first, AD second, evaluated against the user's applicable associations.

| Person or field | Source or responsibility | Boundary |
|---|---|---|
| Maya and Daniel's profiles | Workday; both have Workday and AD associations | One effective profile source per user. |
| Department | Inherits from the effective profile source | Workday for these employees; Okta for Priya. |
| AD-linked employee work email | Explicit AD attribute source | Avoid a competing outbound write back over this AD-owned field. |
| Priya's profile | Okta | No Workday association or AD account assignment; outside employee import scope. |
| Priya's maintenance | Authorized administrator after sponsor approval | A sponsor is a business approver, not another technical source integration. |
| Alex's administrative identity | Okta | Excluded from employee source imports. |
| Employee lifecycle | Configured Workday-driven events | Attribute ownership alone does not define deactivation behavior. |
| AD password validation | AD through the Okta AD agent on that sign-in path | Separate from HR profile ownership and import success. |
| Projects department code | Okta-to-Projects mapping | Sales → SAL; Finance → FIN; IT → IT. Target delivery needs its own evidence. |

An absent field, failed import, or unavailable source does not by itself prove a change in ownership or fallback. Check actual associations, field settings, mappings, and operation results.

[Company reference](northbridge-company.md) · [Course home](../README.md)
