---
title: "Day 1: People, identities, and access"
parent: Revision Guide
nav_order: 1
---

# Day 1 revision: People, identities, and access

Think back to Maya. She can sign in to Okta, but that alone does not tell us whether she can use Salesforce. The useful habit from this day is to separate the facts that sit behind the phrase “she has access.”

## The concepts to keep with you

**Identity and account.** Maya is a person. Her identity information describes her, such as her name and department. An account is a record in a particular system through which she may receive access. Her Workday record, Okta user, AD account, and Salesforce account are separate records about the same person.

**Directory, profile, and attribute.** A directory stores and organizes identity records. Okta's Universal Directory is its directory layer. A profile holds information about a user; each named piece, such as `department`, is an attribute. A correct department in Workday does not prove another system holds the same value.

**Authentication.** This is the check of who is signing in, using an accepted method of proving identity. Maya successfully authenticating to Okta tells us about that sign-in. It does not establish her Salesforce permissions.

**Authorization.** This is the decision about what someone may access or do. Okta application assignment is one access decision. Salesforce permissions are another. Daniel might enter an application but still be unable to approve a request inside it.

**Single sign-on, or SSO.** SSO lets connected applications use a trusted sign-in relationship. It helps the user move into an application without treating every application as a completely separate sign-in. The target still has to accept the information and apply its own access rules.

**Provisioning and deprovisioning.** Provisioning manages application accounts, including creating or updating them. Deprovisioning removes or disables access through the supported account-management process. These are separate from SSO. Also, a connector's account-management capability does not mean it uses SCIM.

## Put the pieces back together

For Maya, keep five statements separate: her Okta user exists, she is assigned Salesforce, her Salesforce account exists, her sign-in succeeds, and her permissions allow the intended work.

The Day 1 search found no account matching her approved identifier **in the checked Salesforce instance**. That is useful evidence with a defined scope. Assignment and provisioning were not yet inspected. We therefore have more to check before deciding that an account must be created.

When you see a tile or an “Active” status, ask which of those five statements it actually supports. Avoid turning one observation into a claim about the whole journey.

## Check your understanding

- Why could Maya authenticate successfully but still lack usable Salesforce access?
- What would you inspect when Daniel can enter an app but cannot approve an item?
- How is “we have not checked the account” different from “our scoped search found no match”?

[Full lesson](../lessons/day-01-people-identities-access.md) · [Exercises](../exercises/day-01.md) · [Answers](../self-checks/day-01.md) · [All recaps](index.md)
