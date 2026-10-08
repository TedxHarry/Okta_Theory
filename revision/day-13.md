---
title: "Day 13: Policies and sessions"
parent: Revision Guide
nav_order: 13
---

# Day 13 revision: Policies and sessions

When someone is prompted again, do not immediately call it an MFA fault. First ask which policy applied, what proof it required, and whether the existing proof was sufficient and recent enough.

## The concepts to keep with you

**Policy and rule.** A policy groups requirements. Its rules use conditions to select an outcome. Check the application's policy association and the actual matching rule. In the taught rule order, the first matching rule applies; a stricter rule lower down does not automatically override it.

**Different policy responsibilities.** Enrollment policy governs registration requirements. Global session policy governs the Okta session. App authentication policy governs authentication requirements for access to the app. The app's own permissions still decide which actions the user may perform inside it.

**Assurance and freshness.** Assurance concerns properties of the authentication proof. Freshness concerns how recent that proof is. Having an enrolled authenticator or an existing Okta session does not prove that an application's current requirement is satisfied.

**Maximum lifetime and idle timeout.** Maximum lifetime limits the total duration of a session. Idle timeout concerns inactivity. Reauthentication requirements concern when fresh proof is needed. These settings answer different questions.

**Okta session and application session.** Okta and Expense can each maintain their own session. Ending one does not inherently terminate the other. Local logout and configured single logout also have different scope; single logout depends on the supported participating relationships.

**Event, transaction, and session identifiers.** An event describes an occurrence. A transaction connects a particular operation, and session identifiers help relate activity within the relevant system. Do not treat an event ID or Okta session ID as Expense's local session ID.

## Revisit Daniel's sign-in

In the first packet, the selected requirement needs possession proof that is not sufficiently fresh. Daniel is enrolled, and a Push challenge is issued, but no accepted proof is recorded. The challenge is consistent with the requirement; enrollment alone does not satisfy it.

The next packet confirms the current proof, OIDC validation, and Expense entry. That supports this successful path, not an application approval permission.

In a separate rule-order packet, an earlier broad rule matches before the intended restrictive rule. Review the order and affected populations. Shared policy changes may affect several applications, so check the full association scope.

## Revisit logout

The session example confirms that Okta session `K-13` can no longer be reused, while Expense session `E-LOCAL-13` still receives a new protected response. The app owner's evidence supports the separate local-session explanation in this case.

That explains a possible mechanism for lingering access, but it does not retroactively prove Jordan's exact cause in Day 12. Also, prompt-free re-entry can create a new app session through a still-valid Okta session. Check identifiers and events before declaring logout failed.

## When a request reaches you

Predict the matching rule and required proof for an eligible user, an excluded user, and an existing session. Verify shared applications after an approved policy change instead of testing only the original reporter.

## Check your understanding

- Why can an enrolled user with an Okta session still need another challenge?
- Why might a restrictive rule never be reached?
- What evidence distinguishes a surviving app session from a newly created one?

<details markdown="1">
<summary>Compare your reasoning</summary>

1. The application's requirement may demand proof that the existing session does not supply, or proof that is more recent. Enrollment alone establishes neither.

2. An earlier matching rule can select the outcome first. Read the actual policy association and rule order rather than assuming the strictest rule wins.

3. Compare the application's session identifiers and creation or termination evidence with the Okta attempt. A new app session can be created without another prompt when a suitable Okta session remains valid.

</details>

[Full lesson](../lessons/day-13-policies-and-sessions.md) · [Exercises](../exercises/day-13.md) · [Lesson exercise answers](../self-checks/day-13.md) · [All recaps](index.md)

[Previous recap: Day 12](day-12.md) · [Next recap: Day 14](day-14.md)
