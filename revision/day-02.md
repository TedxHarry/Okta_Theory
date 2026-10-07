---
title: "Day 2: Requests and evidence"
parent: Revision Guide
nav_order: 2
---

# Day 2 revision: Requests and evidence

When someone says “Okta worked, but the app failed,” slow the story down. Which request worked? Which system returned the failure? Following the individual steps gives you something you can investigate.

## The concepts to keep with you

**Request and response.** A client asks a server to do something, and the server returns a response. In a browser journey, the browser is the client. Other software can also send requests, such as an account-management service calling an API.

**URL.** The scheme, host, path, and query help you identify where a request is going and what information accompanies it. HTTPS protects communication in transit. It does not prove that the user is entitled to access the destination.

**HTTP methods and status codes.** `GET` generally retrieves a resource; `POST` submits information for processing. Read the endpoint and body to understand the actual operation. A `200` response can simply deliver a sign-in form. A `302` with a `Location` header directs the browser elsewhere. A `403` tells you the request was refused, but you still need evidence of the reason.

**JSON and log fields.** JSON organizes data into named values, objects, and lists. Read a field in its context: an event's `outcome.result` describes that event, not every later step. The actor, target, event type, and time help establish what happened.

**Correlation.** This means connecting records that belong to the same attempt. Compare the person, application instance, timestamps and time zones, and relevant identifiers. Two records mentioning Maya are not automatically part of the same sign-in.

**Cookies and sessions.** A cookie is data the browser stores and sends according to its rules. A session represents an ongoing interaction recognized by a system. Neither is the user's account record. Keeping those ideas separate will help when we later examine logout and access removal.

## Follow Maya's evidence

In the Expense example, the browser follows a redirect, receives a sign-in form, and completes authentication. An Okta SSO event reports success. Expense then returns a refusal about the sign-in information.

That sequence places the first observed failure at Expense. It does not yet identify the root cause. A password reset would be a guess when the supplied evidence already shows authentication completed.

Build your explanation around the last confirmed successful step and the next observed failure. Then request evidence from that boundary. This is more useful than collecting unrelated success and failure messages.

## Check your understanding

- Why does receiving a page with status `200` not prove authentication succeeded?
- What does the Okta success event establish in Maya's sequence, and what remains unknown?
- What would you compare before joining two log records into one story?

[Full lesson](../lessons/day-02-requests-and-evidence.md) · [Exercises](../exercises/day-02.md) · [Answers](../self-checks/day-02.md) · [All recaps](index.md)
