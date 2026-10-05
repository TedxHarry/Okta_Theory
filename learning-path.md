---
title: "Learning Path"
nav_order: 3
---

# Learning path

Follow the numbered lessons in order. Each begins with a Northbridge situation, explains the relevant concepts, and asks you to reason from supplied evidence. Try the exercises before reading their separate answers. If an answer exposes a missed distinction, revisit the named lesson section and explain the example again in your own words.

## Build the foundations

| Lesson | Main question | Focus |
|---|---|---|
| [Day 1](lessons/day-01-people-identities-access.md) | Why can someone sign in but still lack application access? | Person and accounts; authentication and authorization; SSO and account management. |
| [Day 2](lessons/day-02-requests-and-evidence.md) | What did this particular request or event establish? | Follow the browser, name the responding system, and keep each result in context. |
| [Day 3](lessons/day-03-profiles-and-mappings.md) | Where did the value change? | Profiles, mapping direction, transformations, and target verification. |
| [Day 4](lessons/day-04-sources-and-ownership.md) | Who controls this person's data? | Source associations, priority, attribute ownership, and contractor maintenance. |
| [Day 5](lessons/day-05-groups-and-assignments.md) | How do those values lead to an application assignment? | Rules, groups, individual exceptions, and independent target checks. |

Directory and profile vocabulary in Day 1 is orientation; the central distinctions matter most. In Day 2, follow the request story before worrying about remembering every code or field name. Use the [glossary](reference/glossary.md) when a term interrupts your understanding.

Before reading an answer, commit to an explanation in your own words. After checking it, change one fact and explain what would change in your conclusion. For example, a correct app user profile with a stale target calls for different evidence from a mapping that produces the wrong value. Day 4's final exercise combines these earlier distinctions with source ownership.

After Day 5, attempt the [foundation checkpoint](assessments/foundation.md). Its [separate debrief and rubric](assessments/self-checks/foundation.md) help identify which distinction to revisit.

## Continue the connected story

| Day | Question |
|---|---|
| [6](lessons/day-06-active-directory.md) | Why can an AD import succeed while AD password sign-in fails? |
| [7](lessons/day-07-authenticators-enrollment-mfa.md) | How do authenticators, enrollment, and MFA requirements differ? |
| [8](lessons/day-08-saml.md) | How does a SAML application accept or reject sign-in information? |
| [9](lessons/day-09-oidc.md) | How does an OIDC application finish sign-in, and what do its tokens mean? |
| [10](lessons/day-10-scim.md) | How are accounts created, updated, and deactivated through SCIM? Includes the [integration checkpoint](assessments/integration.md). |
| [11](lessons/day-11-imports-and-matching.md) | How are discovered accounts matched to existing identities? |
| [12](lessons/day-12-joiners-movers-leavers.md) | What changes when someone joins, moves, or leaves? |
| [13](lessons/day-13-policies-and-sessions.md) | How do policy decisions and separate sessions affect access? |
| [14](lessons/day-14-requirements-and-responsibilities.md) | How do you turn a business request into a clear access requirement? |
| [15](lessons/day-15-integrated-case.md) | Can you explain and investigate the connected Northbridge environment? Includes the [final case](assessments/final-case.md). |

## Read the examples consistently

Maya starts in Sales; her move to Finance belongs to the later lifecycle story. Daniel is already a Finance manager. An explicitly labelled alternative packet changes the evidence for a question; it does not silently change everyone's history.

A lesson or exercise may state that an account exists even when an earlier ticket left its existence unknown. Use the facts of that labelled scenario. Do not treat a hypothetical account or a supplied follow-up as proof of an unshown production event.

Use the [company reference](reference/northbridge-company.md) for people and systems, the [ownership reference](reference/attribute-ownership.md) after Day 4, and [your notebook](notebook/guide.md) to connect each new flow to the earlier ones.

[Course home](README.md)
