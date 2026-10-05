# Day 9: OIDC and the authorization code

Maya opens Northbridge Expense. Instead of returning to her expense page, the sign-in journey stops with a message that the return address is not permitted.

The application uses **OpenID Connect**, or **OIDC**. As with SAML, the application relies on an identity service and must validate what it receives. The messages and their purposes are different.

Start by locating the stage that failed. A rejected return address is not the same problem as an application rejecting a token it has already received.

## OAuth and OIDC answer related questions

**OAuth 2.0** is a framework for authorizing access to protected resources, such as APIs. A client can obtain an access token to present to an intended resource service. That is different from a standard statement establishing who signed in to the client application.

OIDC adds an identity layer to OAuth 2.0. It defines an **ID token** and rules the client uses to validate authentication information about the user.

| Question | Relevant concept |
|---|---|
| How can an application obtain authority to call a protected API? | OAuth and an appropriately issued access token. |
| How can the application verify information about the user's authentication? | OIDC and ID token validation. |

An access token is not a substitute for an ID token merely because it contains readable user information. Neither token automatically creates an application account or grants every application permission.

## Fix the client type before tracing the flow

Northbridge Expense is a fictional web application with a server-side backend. The backend processes sign-in responses and can protect its client credentials. This is a **confidential client**. A **client** is the application participating in the OAuth/OIDC exchange; it is not Maya's user account.

The example uses Authorization Code with PKCE and configured backend client authentication. It does not use a browser-only application or a mobile client as a second architecture.

| Participant | Responsibility in this example |
|---|---|
| Maya and her browser | Start access and carry redirects. |
| Expense backend | Acts as the OIDC client, exchanges the code, validates identity information, and establishes its application session. |
| Okta | Acts as the OpenID Provider and authorization server, applying requirements and issuing the configured responses. |

An **authorization server** issues OAuth responses and tokens. In this OIDC flow it also serves as the **OpenID Provider**. These roles describe this connection, not every role a product might have.

A **client ID** identifies Expense's registration. It is not a password. Backend client authentication uses the separately configured credential or key; that confidential material does not belong in a browser redirect.

## Follow one successful journey

An **authorization code** is a temporary, single-use value that the client exchanges for tokens. It is not an MFA code and not a token for calling an API.

```text
1. Browser → Expense: start sign-in
2. Expense → browser → Okta: authorization request
3. Okta: validate request and perform applicable user/authorization checks
4. Okta → browser → Expense callback: code and returned state
5. Expense backend → Okta token endpoint: exchange code
6. Okta → Expense backend: token response
7. Expense: validate the response and ID token, identify the user,
   and establish the application session if its conditions are met
```

The **callback**, or redirect URI, is the application's registered return location. The **authorization endpoint** receives the authorization request. The **token endpoint** receives the backend's code exchange. An endpoint is an address for a particular operation.

Notice the change at Step 5: the backend communicates with Okta directly. In this server-side flow, the browser carries the code back; the token response goes to the backend. The application session is a further result, not another name for the code or ID token.

The actual authentication interaction depends on the applicable requirements and existing context. Do not assume every request displays a new password prompt or a consent screen.

## Read the request's purpose

Here are selected, decoded parameters from a synthetic successful authorization request. This is a readable field list, not a complete URL or runnable request. All addresses and identifiers are fictional.

| Parameter | Example value | Meaning |
|---|---|---|
| client_id | `nb-expense-web` | The intended Expense registration. |
| response_type | `code` | Request an authorization code. |
| scope | `openid profile` | Request OIDC sign-in and the supported profile-claim category. |
| redirect_uri | `https://expenses.northbridge.example/oidc/callback` | The registered application return location. |
| state | A fresh transaction value, omitted | Correlate the returned browser response with the request. |
| nonce | A fresh transaction value, omitted | Bind the returned ID token to this authentication request. |
| code_challenge / code_challenge_method | A derived value, omitted / `S256` | Establish the PKCE check for the later code exchange. |

`openid` makes this an OIDC request. A **scope** names requested access or information categories. A **claim** is a named statement in returned information, such as a subject identifier. Requesting `profile` does not guarantee every conceivable profile field will appear in every token.

The client must check the returned state against the transaction it started. Because this example sends a nonce, it also checks the ID token's nonce. Matching either value does not replace all other validation.

## What PKCE adds

**PKCE**, Proof Key for Code Exchange, ties code redemption to a value the client created for that transaction. Expense keeps a secret random **code verifier** for the transaction and sends a derived **code challenge** with the authorization request. During redemption, it supplies the verifier, which the authorization server checks against the earlier challenge.

This helps prevent an intercepted or injected code from being redeemed without the required proof. It is not Maya's password. In this confidential-client example, PKCE complements rather than replaces configured client authentication.

The request's `S256` names the method that derives the challenge using SHA-256, a one-way cryptographic calculation, and a URL-safe encoding. The authorization request carries that derived challenge, while the backend retains the verifier for the later exchange. See the [PKCE specification](https://www.rfc-editor.org/rfc/rfc7636.html#section-4.2).

You do not need to calculate the challenge to investigate the flow. Recognize that a code arriving at the callback is only an intermediate step; the exchange has its own requirements and outcome.

## Three artifacts, three purposes

A successful token response in this example includes an ID token and an access token. This synthetic JSON shows selected field names only. The strings are explanatory placeholders, not real or encoded tokens:

```json
{
  "id_token": "ID_TOKEN_PLACEHOLDER",
  "access_token": "ACCESS_TOKEN_PLACEHOLDER",
  "token_type": "Bearer"
}
```

| Artifact | Used by | Purpose |
|---|---|---|
| Authorization code | Expense backend at the token endpoint | Obtain the token response under the exchange requirements. |
| ID token | Expense as the OIDC client | Validate information about the user's authentication. |
| Access token | Its intended resource server | Authorize an API request after the relevant token and permission checks. |

**Bearer** means possession of the token is sufficient to present it for use, subject to validation. Treat it as sensitive. Its existence does not show that any API call succeeded.

This sign-in example does not add a custom Expense API or assume that an Okta-issued access token is valid for an arbitrary API. The intended resource, issuer configuration, scopes, and validation contract determine where it can be used.

## Read claims without claiming validation

An ID token uses **JWT**, JSON Web Token, to represent claims. A readable decoded payload reveals content; it does not prove authenticity or acceptance. The following selected claims are a synthetic illustration, not a complete ID token:

```json
{
  "iss": "https://identity.northbridge.example",
  "sub": "subject-maya-1042",
  "aud": "nb-expense-web"
}
```

`iss` identifies the issuer. `sub` identifies the subject within that issuer. `aud` identifies the intended audience; for this single-audience ID token, it is Expense's client ID. Northbridge associates the validated issuer/subject pair with the intended local account, rather than assuming email is an immutable global identifier.

The client checks the expected issuer and audience, validity such as expiration, nonce for this request, and the applicable cryptographic validation using trusted provider information. Signature verification requires the actual token and appropriate trusted keys; looking at decoded JSON is insufficient. The excerpt omits signature and other required claims, including expiration and issuance information.

Keep validation at this purpose level: who issued it, who it describes, who it is intended for, and whether it is valid for this transaction. A JWT-shaped string is not automatically an acceptable ID token or access token.

## Find the redirect mismatch

Return to Maya's incident O-1. The investigator has matched these observations to the same authorization request and registration:

| Evidence | Observation |
|---|---|
| Approved Expense callback | `https://expenses.northbridge.example/oidc/callback` |
| Registered sign-in redirect URI | Exactly the approved callback above. |
| Actual request redirect_uri | `https://expenses.northbridge.example/oidc/callback-old` |
| Request validation result | Rejected because the requested redirect URI is not registered. |
| Code issued for O-1 | None. |
| Code exchange for O-1 | Did not occur. |

The first demonstrated defect is the stale callback in the request. The registration matches the approved design; the application's request does not. Exact redirect matching prevents sending codes to unapproved destinations. An invalid redirect should not be used as the destination for sending the error back.

Propose correcting the application configuration that emits the old callback, then verify a new request. Do not add an obsolete address merely to suppress the rejection, broaden destinations indiscriminately, or reset Maya's authenticator.

O-1 has no issued code or token exchange. Searching for the cause in an ID token from another attempt would cross an evidence boundary. A token-audience problem belongs to a later stage that this attempt did not reach.

## Verify the correction through the remaining stages

For a new attempt O-2, the investigator records:

1. The request uses the approved registered callback and is accepted.
2. After the applicable checks, a code returns to that callback; Expense verifies state.
3. The backend completes the authenticated code exchange with its matching PKCE verifier.
4. Expense validates the returned ID token, including its expected nonce, and identifies Maya's intended account.
5. Expense establishes its application session and Maya enters the application.

These observations support successful application entry for O-2. They do not establish that Maya can approve expenses or that a provisioning operation occurred. Earlier distinctions still apply even though the protocol changed.

## Compare with SAML

| SAML example from Day 8 | OIDC example here |
|---|---|
| Browser carries a response containing an assertion to the ACS. | Browser returns a code to the callback; backend exchanges it for tokens. |
| SP validates the assertion under its trust configuration. | Client validates the ID token and transaction under its OIDC configuration. |
| NameID is interpreted through the configured matching rule. | Expense uses the validated issuer/subject identity for its account association. |

Both involve trusted identity information and application-side validation. Their messages and validation rules are not interchangeable. Neither protocol alone proves account creation or complete application authorization.

## Before moving on

Can you explain:

- How OAuth and OIDC relate?
- Which steps use the browser and which use Expense's backend?
- Why the code, ID token, and access token have different purposes?
- Why a scope is different from a claim?
- What state, nonce, and PKCE protect at a purpose level?
- Why decoded claims do not establish token validity?
- Why O-1 should be investigated before token processing?

Try the [Day 9 exercises](../exercises/day-09.md), then compare with the [answers](../self-checks/day-09.md). The optional final comparison in the answer file considers a later token rejection.

In [your notebook](../notebook/guide.md), draw the browser and backend as separate participants. Mark the last completed stage before proposing the next evidence request.

[Day 10](day-10-scim.md) returns to account management through SCIM.

[Previous: Day 8](day-08-saml.md) · [Course home](../README.md)
