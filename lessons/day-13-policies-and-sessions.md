---
title: "Day 13: Policies, sessions, and another authentication check"
parent: Lessons
nav_order: 13
---

# Day 13: Policies, sessions, and another authentication check

Daniel opens Northbridge Expense from his Okta dashboard and receives another authentication challenge. He asks, “I am already signed in. Why am I being asked again?”

An existing session and an acceptable authentication result for this application are different facts. The challenge could be expected, or it could reveal an incorrect rule or context. Alex needs the applicable requirement and the evidence for this attempt before deciding.

Daniel remains an active Finance manager. His Expense assignment and existing application account are confirmed for this lesson. Missing assignment and missing provisioning are not the demonstrated problems.

## Give each policy a specific job

A **policy** groups requirements. A **rule** states when particular conditions apply and what decision or authentication requirement follows.

| Policy or control | Main question |
|---|---|
| Authenticator enrollment policy | Which authenticators may or must this person enroll? |
| Global session policy | What requirements and limits govern the Okta session? |
| Application authentication policy, also called an app sign-in policy | What authentication must satisfy this application's access request? |
| Application's own permissions | What may the person do after entering the application? |

Enrollment is not an accepted challenge response. An app sign-in policy is not the app's expense-approval role. Okta Identity Engine requires the applicable global-session and app authentication requirements to be satisfied; one does not simply cancel the other. See [Okta policies and rules](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/about-policies.htm).

**Authentication assurance** means the confidence supported by the authentication evidence and its characteristics. A requirement can concern factor types, acceptable methods, or properties such as phishing resistance. “MFA happened” is too vague if the current requirement demands something that evidence does not satisfy.

## Keep session lifetime and reauthentication separate

A **session** lets a service recognize an ongoing interaction after authentication. In this browser-based example, cookies help the browser present session information to the relevant service. A cookie's presence alone does not prove that the server still accepts the session.

The global session policy governs the Okta session, including its maximum lifetime and idle limit. A maximum lifetime limits overall duration; an idle limit concerns inactivity as evaluated by that service. Neither should be assumed to equal the lifetime of an application's own session.

**Reauthentication** is another authentication check required for an access request. The app sign-in policy can require renewed evidence even while an Okta session remains valid. Existing evidence might also be reusable when it satisfies the applicable requirements. The result depends on the actual rule and context, not a universal “one prompt per session” rule. See [global-session rules](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/add-okta-sign-on-policy-rule.htm) and [app sign-in rules](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/add-app-sign-on-policy-rule.htm).

An Okta app sign-in rule acts when the application sign-in flow reaches Okta. It does not mean every click within an already established application session automatically returns to Okta for evaluation.

## Identify the policy that actually applies

An application must be associated with the intended authentication policy. A policy may be shared by multiple applications. Editing a shared rule can therefore affect more than the application named in the ticket.

For the ordered rules used here, Okta checks rule conditions in priority order. The matching rule determines the action; a more restrictive rule placed lower does not automatically win. Conditions may use group membership, device information, or network context. Inspect the values evaluated for this attempt, not just the user's usual location or a policy's descriptive name. See [rule evaluation order](https://help.okta.com/oie/en-us/content/topics/identity-engine/devices/add-app-signon-policy-desktop.htm).

These policy priorities are different from Day 4's profile-source priority. One selects authentication behavior; the other concerns data ownership.

## Follow Daniel's expected challenge

Packet P-1 uses simplified policy intent, not literal Admin Console labels or a complete configuration. Daniel has already completed the required enrollment from Day 7. Expense uses Day 9's confidential OIDC web-client architecture.

| Evidence | Observation |
|---|---|
| Okta session K-13 | Present and valid for Daniel in the checked browser. |
| Expense policy association | The intended Expense authentication policy is assigned. |
| Matched rule E-13 | The configured rule applies to Daniel's observed group and context. |
| Rule requirement | Password plus an allowed possession factor, with separate configured freshness requirements. |
| Evaluation of existing proof | Password evidence remains acceptable; the possession-factor freshness condition is not satisfied. |
| Enrollment | Daniel has an eligible Northbridge Okta Verify enrollment. Push is allowed in this example. |
| Current result | A Push challenge is issued for the Expense sign-in. No accepted response has yet been recorded. |

**Freshness** asks whether previous authentication is recent enough under the applicable rule. This packet explicitly supplies the evaluation result; the reader does not need to calculate an interval or assume that all factors share one interval.

The challenge is consistent with the stated rule. Daniel's valid Okta session did not make older possession evidence sufficient. His enrollment supplies a possible method, not proof that he completed this challenge.

There is no demonstrated policy defect in P-1. Explain the requirement and investigate the challenge outcome if he cannot complete it. Removing the requirement just to eliminate a prompt would change the approved access decision.

In follow-up P-2, the current Push response is accepted and Okta records that the applicable authentication requirements are satisfied. Expense's backend then completes its OIDC exchange and validation, associates Daniel with his intended account, and establishes application session E-LOCAL-13. A protected page is returned successfully.

P-2 demonstrates entry for that attempt. It does not prove that Daniel may approve a particular expense, nor does it prove that an unrelated earlier failed attempt succeeded.

## Contrast a genuine policy-selection problem

Independent packet P-3 changes the policy evidence. The approved design places a restrictive rule for the identified sensitive population before a general rule. The observed configuration instead has a broad matching rule above it. The evaluation record shows that the broad rule matched Daniel and did not require the intended additional proof.

The discrepancy is between the approved rule order and the applied configuration. Inspect the policy's application associations, rule conditions, priority, and affected populations. Propose restoring the approved selection behavior, then verify both a relevant request and a request that should follow a different rule.

Do not diagnose P-1 using P-3's evidence: P-1 correctly selected its rule. Conversely, “Daniel got in” would not prove P-3 meets the intended protection requirement. Success must be judged against the approved behavior.

## Build a correlated evidence trail

The **System Log** records Okta-side events. Its outcome belongs to a particular action. An authentication success, session creation, and app SSO outcome answer different questions.

Start with the person, application instance, relevant sequence, and observed behavior. Then connect related records using identifiers where available:

| Field or evidence | Use |
|---|---|
| Actor and target identifiers | Identify who or what acted and which objects were affected. |
| Event type and outcome | Establish the action and its recorded result. |
| Event uuid | Identify an individual event. |
| transaction.id | Relate events within an operation. |
| authenticationContext.externalSessionId | Relate events within an Okta user session. |
| authenticationContext.rootSessionId | Group sessions sharing a common root where that context is available. |
| Application's own evidence | Establish its protocol validation, account association, local session, or permission result. |

These fields are described in [Okta's System Log correlation guide](https://developer.okta.com/docs/reference/system-log-query/). The name externalSessionId does not make it the application's own session identifier. One browser journey may involve several transactions; some records lack a usable session identifier. Do not force every event into a single transaction or discard a relevant event solely because its session field is absent.

For P-1/P-2, an investigator's synthetic summary is:

```text
Daniel + Expense + Okta session K-13
    operation A: applicable rule; renewed possession evidence required
    operation B: current challenge response accepted
    operation C: Okta-side Expense SSO result
Expense's correlated records
    OIDC exchange and validation accepted
    intended Daniel account selected
    local session E-LOCAL-13 established; protected request accepted
```

The labels above are teaching identifiers, not raw Okta event values. The investigator has correlated the operations and application records; their identifiers are not assumed to be identical across systems.

A nearby success for Maya, another app, or an earlier Daniel attempt is not a substitute. Likewise, a search returning no event may reflect incomplete coverage rather than proof that nothing happened. Day 2's evidence boundaries and Day 11's completeness checks still apply.

## Explain why two sessions matter

After P-2, the browser can interact with two services:

```text
Browser → Okta: presents information for Okta session K-13
Browser → Expense: presents information for application session E-LOCAL-13
```

Expense established its own session after validating the sign-in exchange. In this fictional implementation, ordinary protected-page requests are checked against Expense's local session, not sent to Okta for a fresh OIDC exchange every time. That behavior must be established from the application, not assumed for all products.

An ID token is not the same object as that local session. Expiration of an ID token does not by itself demonstrate that the application ended a session it previously created. Similarly, active false in an account-management response is not a session-termination receipt.

Consider packet S-13, after an approved session-ending action for Daniel. His employment and application assignment are unchanged:

1. Okta confirms that K-13 is ended.
2. A subsequent request to Okta cannot reuse K-13 and requires the applicable sign-in process.
3. Expense still accepts a new protected request through E-LOCAL-13.
4. Expense's owner confirms that its local session remains valid and this request did not initiate a new Okta sign-in.

The observations establish different outcomes in the two systems. Ending K-13 worked; it did not end the checked Expense session. This is not evidence that Okta deactivation failed, because no deactivation was performed in S-13.

## Define what signing out must accomplish

**Local logout** ends the application's session under its implementation. It does not automatically establish that Okta or another application's session ended. If an Okta session remains, a later app sign-in may reuse acceptable authentication evidence; a prompt-free return does not necessarily mean the local logout failed.

**Single Logout (SLO)** coordinates logout across participating integrations when supported and configured. Do not infer participation, delivery, or successful termination from the name alone. Ask which sessions an action targets and inspect its results. See [Okta's SLO support and configuration boundaries](https://help.okta.com/oie/en-us/content/topics/apps/apps_single_logout.htm).

For S-13, the appropriate next decision depends on the requirement. If only the Okta session needed to end, the supplied Okta evidence supports that outcome. If both Okta and Expense access through the existing sessions needed to end, the Expense owner must complete and verify its supported termination action. A repeat protected request should no longer succeed through the terminated local session; a newly established session must be distinguished from reuse of the old one.

This also explains the investigation required for Jordan in Day 12. A separate application session is a possible reason access can persist, but Daniel's evidence does not prove Jordan's root cause. Correlate Jordan's actual account and session records, confirm the removal requirement, and verify his outcome independently.

## Before moving on

Can you explain why P-1 is an expected challenge but P-3 is a demonstrated configuration problem? Can you identify which service owns each session, distinguish an event from a transaction, and state what a successful Okta-side result still leaves for the application to verify?

Try the [exercises](../exercises/day-13.md), then the [answers](../self-checks/day-13.md). In [your notebook](../notebook/guide.md), record the expected requirement, actual matched rule, authentication evidence, Okta-session result, and separate application result.

[Day 14](day-14-requirements-and-responsibilities.md) turns business requests into clear access requirements and administrative responsibilities.

[Previous: Day 12](day-12-joiners-movers-leavers.md) · [Course home](../README.md)
