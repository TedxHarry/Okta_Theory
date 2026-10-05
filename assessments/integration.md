# Integration checkpoint: Sign-in and account management

Use Days 1–10 to explain the following independent synthetic packets. Write your conclusions before opening the [debrief](self-checks/integration.md). Each packet names its own application, attempt, and target evidence. Do not transfer success from one application to another.

## Packet A: Salesforce rejects a SAML audience

Daniel has an approved Salesforce assignment and an existing active Salesforce account. For attempt A-1, Okta's record reports a successful SAML response issuance. These are selected fields from that response, not a complete assertion:

| Field | Value |
|---|---|
| Issuer | `https://identity.northbridge.example/saml/idp` |
| Audience | `https://salesforce.northbridge.example/entity/test` |
| NameID | `daniel.brooks@northbridge.example` |

The checked production Salesforce connection expects audience `https://salesforce.northbridge.example/entity/prod`. Its correlated validation report accepts the signature and validity window but rejects the audience. No Salesforce session is established for A-1.

**Task 1:** Categorize the exchange, trace the successful boundary followed by failure, and identify the demonstrated mismatch. Does an existing account or successful Okta response prove application entry? Propose a correction and verification.

## Packet B: Expense accepts OIDC

For Maya's attempt B-1, Expense uses Day 9's confidential backend client. Selected authorization-request fields are:

| Field | Value |
|---|---|
| client_id | nb-expense-web |
| response_type | code |
| scope | openid profile |
| redirect_uri | `https://expenses.northbridge.example/oidc/callback` |

The callback is registered. The backend verifies state, completes the authenticated PKCE code exchange, and validates the ID token, including nonce and the required time and cryptographic checks. These are selected decoded claims, not independent validation evidence:

```json
{
  "iss": "https://identity.northbridge.example",
  "sub": "subject-maya-1042",
  "aud": "nb-expense-web"
}
```

The application confirms Maya's intended pre-existing Expense account, establishes its session, and displays her expense page. Expense-approval permission has not been tested.

**Task 2:** Categorize the exchange. Explain the browser/backend distinction and why the supplied validation report matters beyond the readable claims. State the precise successful outcome. Did this prove account creation?

## Packet C: Projects rejects a create

Maya is also assigned to Projects. Her app profile contains the correct userName and SAL department code. The authorized connector sends a POST to the checked Projects `/scim/v2/Users` endpoint with those values and active true. Projects replies HTTP 409 with:

```json
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],
  "status": "409",
  "scimType": "uniqueness",
  "detail": "userName is already in use in this Projects instance."
}
```

No conflicting record, ownership confirmation, successful association, or follow-up account read is supplied.

**Task 3:** Categorize the exchange and locate its failure. What does the response report, and what remains unknown about Maya's target account? Explain why Packet B does not settle this incident. Request specific evidence before choosing a correction.

## Packet D: Same user, different identity evidence

A colleague suggests using Maya's Expense ID token to authorize the Projects connector, then removing the uniqueness check if the create still fails.

**Task 4:** Explain both errors in that proposal. Name a justified next action using Packet C, without writing a configuration procedure.

## Give a useful handoff

**Task 5:** Write a short handoff for each application's owner: demonstrated outcome, first proven failure if any, next evidence or action, and what would confirm resolution. For every application, state whether the supplied evidence establishes the intended account's existence and actual entry.

Use the [debrief and rubric](self-checks/integration.md) after attempting all tasks. Record one corrected distinction in [your notebook](../notebook/guide.md).

[Day 10](../lessons/day-10-scim.md) · [Course home](../README.md)
