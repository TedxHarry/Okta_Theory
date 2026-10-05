---
title: "Cheat sheets"
parent: Reference
nav_order: 7
---

# Cheat sheets

Quick reminders you will reread often. Use them when a term trips you up or when you want to double-check yourself before you act.

## Easily confused pairs

| This | Is not this | The difference in one line |
|---|---|---|
| Identity | Account | A person is one identity; an account is one system's record of them. |
| Authentication | Authorization | Who is signing in, versus what they are allowed to do. |
| SSO | Provisioning | Signing in, versus creating and managing the account. |
| Assignment | A usable account | Okta intends them to have it, versus the account actually existing and working. |
| Group | Group rule | A collection of users, versus the rule that decides who is in it. |
| Import | Provisioning | Reading accounts into Okta, versus pushing accounts out to an app. |
| SAML | OIDC | Two different sign-in protocols; their messages are not interchangeable. |
| ID token | Access token | Proof of who signed in, versus permission to call an API. |
| Okta session | Application session | Being signed in to Okta, versus being signed in to the app. |
| Profile source | Mapping | Who controls a value, versus how a value moves and changes shape. |
| Agent | Connector | Software in your network that reaches a system, versus the built-in integration to it. |

## Okta user states, in plain words

| State | What it means |
|---|---|
| Staged | The account is created but not active yet. The person cannot use it. |
| Active | The person can use Okta normally. |
| Suspended | Access is temporarily blocked. It can be restored later. |
| Deactivated | Access is removed. The account can be reactivated if needed. |
| Locked out | A temporary condition, usually after too many failed sign-ins. |
| Password reset or expired | A temporary condition until the person sets a new password. |

The first four are lifecycle states. The last two are temporary conditions on an otherwise active account.

## What common evidence proves, and does not

| You see | It proves | It does not prove |
|---|---|---|
| Okta sign-in success | They authenticated to Okta for that attempt | That the app let them in, or that an account exists |
| An app tile on the dashboard | The app is shown to the user | That the account exists or that sign-in works |
| An assignment in Okta | Okta intends them to have the app | That the account was created in the app |
| A SCIM "201 Created" | The app created the account for that request | That sign-in works or that permissions are correct |
| An import finished | Records were read into Okta | That password sign-in works, or that matching is correct |
| A "200 OK" on a page | The request was handled | That the person authenticated |

Keep the right-hand column in mind. Most wrong conclusions come from treating one green result as proof of the whole flow.
