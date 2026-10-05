---
title: "Troubleshooting playbook"
parent: Reference
nav_order: 5
---

# Troubleshooting playbook

Keep this page open when you work a real ticket. It does not replace the lessons. It gives you a starting path for the most common problems, and it reminds you what each check actually proves.

Start every ticket the same way:

1. What should have happened for this person and this app?
2. What actually happened? Get the real symptom, not a vague "it does not work."
3. Where did expectation and reality first differ?
4. What evidence proves it, and what does that evidence not prove?

Then pick the matching path below. Each path tells you where to look first, not what to change blindly. Confirm the cause with evidence before you fix anything.

## The person cannot use an application

```mermaid
flowchart TD
  S["Person cannot use the app"] --> Q1{"Is the app assigned<br/>to them in Okta?"}
  Q1 -->|No| A1["Check the assignment path:<br/>group rule, group membership, or direct assignment"]
  Q1 -->|Yes| Q2{"Does an account exist<br/>in the target app?"}
  Q2 -->|No| A2["Check provisioning result,<br/>or how accounts are created for this app"]
  Q2 -->|Yes| Q3{"Can they sign in<br/>to the app?"}
  Q3 -->|No| A3["Check federation (SAML or OIDC)<br/>and the sign-in policy"]
  Q3 -->|Yes| A4["Check the role or permission<br/>inside the application itself"]
```

Remember: an Okta sign-in success only proves they reached Okta. It does not prove the app account exists or that the app let them in.

## The person is not getting the application

```mermaid
flowchart TD
  S["Expected app is missing"] --> Q1{"Do their profile values<br/>match the rule? (department, type)"}
  Q1 -->|No| A1["Fix the source data or mapping<br/>that feeds those values"]
  Q1 -->|Yes| Q2{"Are they in the group<br/>the rule should add them to?"}
  Q2 -->|No| A2["Check the group rule logic<br/>and whether it has evaluated"]
  Q2 -->|Yes| Q3{"Is the app assigned<br/>to that group?"}
  Q3 -->|No| A3["Assign the app to the group,<br/>or find the intended assignment path"]
  Q3 -->|Yes| A4["Check provisioning, since assignment<br/>is not the same as an account"]
```

## Assigned in Okta, but no usable account in the app

```mermaid
flowchart TD
  S["Assigned, but no account"] --> Q1{"Is provisioning configured<br/>for this app?"}
  Q1 -->|No| A1["Accounts may be created another way.<br/>Find the responsible process."]
  Q1 -->|Yes| Q2{"What did the provisioning<br/>attempt report?"}
  Q2 -->|Error| A2["Read the exact response.<br/>A conflict means a name is in use;<br/>a bad value means a mapping is wrong."]
  Q2 -->|No attempt| A3["Check whether the assignment<br/>actually triggered provisioning"]
  Q2 -->|Success| A4["Check the target account state<br/>and whether it matches this person"]
```

## Moved department, but old access remains

```mermaid
flowchart TD
  S["Department changed,<br/>old access still there"] --> Q1{"Did the new department<br/>reach the Okta profile?"}
  Q1 -->|No| A1["Check the source and mapping first"]
  Q1 -->|Yes| Q2{"Was the old access<br/>given by a group rule?"}
  Q2 -->|Yes| A2["Rule-based access should drop<br/>when they no longer match. Verify it did."]
  Q2 -->|No| A3["Directly granted or separately given access<br/>does not disappear on its own. Review it."]
```

## Left the company, but still has access

```mermaid
flowchart TD
  S["Terminated, but access remains"] --> Q1{"Is the Okta account<br/>deactivated or suspended?"}
  Q1 -->|No| A1["Check why the leaver action<br/>did not change the Okta state"]
  Q1 -->|Yes| Q2{"Did each app account<br/>get disabled?"}
  Q2 -->|No| A2["Deprovisioning depends on each app.<br/>Check each one separately."]
  Q2 -->|Yes| Q3{"Is an existing app session<br/>still active?"}
  Q3 -->|Yes| A3["A live session can outlast deactivation.<br/>Check the app's session behavior."]
  Q3 -->|No| A4["Confirm there is no second<br/>access path you missed."]
```

## The application rejects the sign-in

```mermaid
flowchart TD
  S["Okta says success,<br/>app rejects sign-in"] --> Q1{"SAML or OIDC?"}
  Q1 -->|SAML| B1{"Does the audience,<br/>signature, and timing check out?"}
  B1 -->|No| B2["Fix the setting that is wrong.<br/>Do not disable validation."]
  B1 -->|Yes| B3["Check whether the NameID<br/>matches a user in the app"]
  Q1 -->|OIDC| C1{"Which stage failed?"}
  C1 -->|Redirect| C2["The requested redirect URI<br/>is not registered. Fix the app config."]
  C1 -->|Token| C3["Check issuer, audience, and expiry.<br/>Reading a token is not validating it."]
```

## An AD-linked user cannot sign in

```mermaid
flowchart TD
  S["AD user cannot sign in"] --> Q1{"Was the account<br/>imported into Okta?"}
  Q1 -->|No| A1["Check import scope and matching"]
  Q1 -->|Yes| Q2{"Is this an AD password<br/>sign-in?"}
  Q2 -->|Yes| A2["Import success does not prove the password path.<br/>Check the AD agent and domain controller."]
  Q2 -->|No| A3["Check the authenticator and policy<br/>for the method actually used"]
```

A last reminder. "Not checked yet" is not the same as "not there." If you have not looked at the assignment, do not report it as missing. Look, then report what the evidence shows.
