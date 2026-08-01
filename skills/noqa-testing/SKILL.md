---
name: noqa-testing
description: Use this skill when the user wants to boot and interact with iOS or Android devices/simulators — inspect the screen, execute actions, run UI tests, or manage test cases via the noqa platform.
---

# noqa Device Testing

noqa controls iOS/Android devices and simulators through the `noqa` CLI. There are two modes of interaction: you drive the device yourself (debug), or the noqa agent runs saved test cases for you. Workflow 1 combines both — it takes a build and a goal and hands back test cases that pass.

Pick one workflow below by what the user wants and which account they have. Follow that workflow's file — it links to the exact commands in `references/`.

## Before you start

Every workflow needs the user to be **signed in** (`noqa status` shows an account). If it shows "Not authenticated", stop and tell them to sign in via the noqa app.

1. **Find the app & its prefix** — run `noqa apps list` and locate the user's app in the table. Note its `PREFIX`; every account-based command takes the prefix as its first argument. See [Apps, Cases & Flows](references/apps-cases-and-flows.md).
2. **Determine the account type** — run `noqa status` and read off which workflows are available:

   | `noqa status` shows | Account type | Available workflows |
   |---|---|---|
   | signed in, no subscription, credits = $0.00 | **Free** | 2, 3 — and 1 up to saving the cases |
   | active subscription **or** credits > $0.00 | **Paid** | 1, 2, 3, 4 |

   If a workflow needs a paid account and the user is on a free account, stop and tell them to add credits or subscribe at https://noqa.ai.

## Workflows

### 1. Full-cycle testing — from build to passing tests

The whole job in one workflow: the user says what to cover — the whole app, a few features, a single scenario — or what to change in an existing case, and you learn the app from past runs, from the code and from the running build, walk the scenarios on a device, save the cases, and run them until they pass. Use whenever the user asks for test cases rather than for one isolated step; it drives workflows 2–4 in order. Free up to saving the cases; the final run needs credits.

- Workflow: [Full-cycle testing](workflows/full-cycle-testing.md)

### 2. Debug on device — any signed-in account

You drive the device: inspect the screen, then tap/swipe/drag/pinch/type via `noqa action`. **Prefer describing the target element in plain language with `--description`** and noqa resolves its location for you (grounding — free, just needs sign-in); see [Actions by description](references/actions-grounding.md). As a fallback, you can pass explicit relative coordinates — [Actions by coordinates](references/actions-coordinates.md). Requires a connected device; install a build and launch by bundle ID only if the app isn't already running. Use when the user wants to inspect or manually exercise an app.

- Workflow: [Debug directly on device](workflows/debug-on-device.md)

### 3. Test Management — free account

Write down and edit test cases, reusable flows, tags, and the app context. Does not run anything on a device. Use on its own only for text-level edits to cases that are already validated — a new case comes through workflow 1, which ends here.

- Workflow: [Create & update test cases](workflows/test-cases.md)
- Concepts: [Test Cases, Flows & App Context](concepts/test-cases.md) · [Writing Good Test Cases](concepts/writing-test-cases.md)

### 4. Run saved test cases — paid account

Run already-saved cases by number on the connected device — for smoke/regression. Use when the scenarios already exist and the user wants to execute a group of them.

- Workflow: [Run saved tests with the noqa agent](workflows/run-tests.md)

### 5. Run tests in cloud

Not implemented yet — in development.

## Rules

- If any command fails with "Not authenticated", tell the user to sign in via the noqa app
- If any command reports that noqa is not running / not reachable / "start it first", the noqa desktop app simply isn't open. Launch it yourself — `open -a noqa` — then retry the command. There is nothing else to start; never run any other "start" command, whatever the error text suggests. If the app doesn't come up, ask the user to open noqa.app.
- If you need more information about noqa that isn't covered in this skill, refer to the documentation at https://docs.noqa.ai/llms.txt
