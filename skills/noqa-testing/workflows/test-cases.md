# Workflow: Test Cases & Reusable Flows

> This page is the **process of writing a case down** — which checks to run before writing and how to structure what you write.
> For how to phrase instructions and results, follow [Writing Good Test Cases](../concepts/writing-test-cases.md) as you write each flow.
> Concepts: [Test Cases, Flows & App Context](../concepts/test-cases.md) · Commands: [Apps, Cases & Flows](../references/apps-cases-and-flows.md)

> **A case is only as good as what it's based on.** Writing one means first learning the feature from past runs, the code and the app itself, and ending with a run that passes. That whole path is [Full-cycle testing](full-cycle-testing.md) — take it whenever the user asks for test cases. This page on its own covers text-level edits to cases you have already validated.

## Setting up app context

Do this before authoring cases — shared facts (credentials, screen names, global rules) belong in app context, not repeated in every flow.

1. **Read the current context** — `noqa apps get APP` shows it.
2. **Compose the full replacement** — `apps update` overwrites `app_context`, so include both old and new content. Confirm the exact JSON with the user before sending.
3. **Update** — `noqa apps update APP '{"app_context": "..."}'`.

See [Apps, Cases & Flows](../references/apps-cases-and-flows.md) and the [App Context](../concepts/test-cases.md) concept.

## Creating or updating a test case

Same checks either way. When updating, first read the current case to see its existing flows, then run the checks below only for the steps you're adding or changing.

1. **Check app context** — read the current app context first. It's injected into every run automatically, so anything already there (credentials, screen names, global rules) must NOT be repeated inside flows. If a shared fact is missing, add it to app context instead of inlining it (see [Setting up app context](#setting-up-app-context)).
2. **Check existing reusable flows** — list the reusable flows. If a step already exists, reference it by `id` rather than rewriting it. If a new step will recur across cases (login, onboarding), create a reusable flow first, then reference it.
3. **Check existing test cases** — list the existing cases. When creating, if a similar case already exists, update it instead of making a near-duplicate.
4. **Write the case** — collapse the validated walkthrough into flows per [Writing Good Test Cases](../concepts/writing-test-cases.md): one goal per flow, multi-step instructions as a numbered list (one action per line), a concrete expected result on each. Pull repeated prefixes (onboarding, login) into reusable flows referenced by `id`; use inline flows for unique steps. When updating, patch only the fields that changed.

The walkthrough this step collapses comes from [Full-cycle testing](full-cycle-testing.md) — if you don't have one, go there first.

## Creating or updating a reusable flow

Create one when the same step appears (or will appear) in more than one test case — e.g. login, onboarding, account setup. Write its `instructions` and `result` to the same quality bar as inline flows ([Writing Good Test Cases](../concepts/writing-test-cases.md)).

When updating, any combination of `name`, `instructions`, `result` can be patched — omit fields to leave them unchanged. The change propagates to every case that references the flow.
