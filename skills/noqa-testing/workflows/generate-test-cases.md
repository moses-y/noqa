# Workflow: Generate Test Cases

The complete path from a build and a goal to saved test cases that are proven to pass on a device. The user says what they want covered — the whole app, a few features, or a single scenario — and you do the rest: learn the app from what the account already knows about it, from its code, and from the running build, draft the scenarios, walk each one on a device, save it, then run it with the noqa agent until it passes.

Changing an existing case — new steps, a new feature inside it — goes the same way: read what's there, adjust, verify, run.

> **Account:** everything up to and including saving cases works on a **free** account. The last step — running the cases with the agent — needs a **paid** account. If the user is on a free account, do steps 1–5 and tell them the suite is ready to run once they add credits.

> Concepts: [Test Cases, Flows & App Context](../concepts/test-cases.md) · [Writing Good Test Cases](../concepts/writing-test-cases.md) · Commands: [Apps, Cases & Flows](../references/apps-cases-and-flows.md)

## Steps

### 1. Agree what to cover

Turn the user's request into a short, explicit list of scenarios before doing anything else.

- **Broad request** ("cover the main flows", "test my app") — you propose the list. Work out what the app does from step 2 first, then come back with the scenarios you intend to write. Get the user's OK on the list before writing anything.
- **Specific request** ("cover the profile screen") — still look around the target: adjacent screens, entry points, and states that the scenario has to pass through. A case that only covers the happy path of one screen usually misses the setup it depends on.

Keep the first batch small enough to finish end to end. It is better to hand back three cases that pass than ten that were never run.

### 2. Gather context

Cheapest source first, and stop once you can write precise steps. Commands: [Apps, Cases & Flows](../references/apps-cases-and-flows.md) · [Run Tests](../references/run.md).

- **App context** — credentials, screen names, global rules injected into every run. Read it first: anything already there must **not** be repeated inside flows, and a shared fact that is missing goes here rather than into every case — [how to set it](../concepts/test-cases.md#setting-up-app-context).
- **Existing cases** — update a similar case instead of creating a near-duplicate. Note the ones near your feature; you'll read their runs next.
- **Reusable flows** — reference by `id`; if a step will recur (login, onboarding), create the flow first.
- **Runs of those cases** — the richest source. Read a few in detail: flows, expected results, executed steps with screenshots give you the screens, their names and the transitions between them, from the user's side. Usually answers most of what you'd otherwise walk the app for. Empty on a fresh account — then lean on the two sources below.
- **The code** — screens, navigation, business rules, edge cases, feature flags. Read the source files behind the feature; you can't write precise steps for a screen you haven't looked at.
- **The running app** — only where the runs and the code leave you guessing: a screen nobody has tested, a state you can't predict from the source. Get a build, install it, and explore via [Device control](device-control.md).

**The build:** use the binary or the build command the user gave you; otherwise work it out from the repo (README, build scripts, CI config) and build it: [iOS Native](../references/builds-ios-native.md) · [React Native](../references/builds-react-native.md) · [Android](../references/builds-android.md). If neither the user nor the repo says how, ask.

If the app's purpose still isn't clear anywhere, write down a short summary of what it does and what its main features are as you explore — that belongs in app context.

### 3. Draft the steps

For each scenario, write the steps you expect to perform, starting from a **cold launch on a clean install** — every saved run reinstalls the app, so the path must include permissions, onboarding, and login. This is still a draft: its job is to be checked against the device in the next step.

Agree the draft with the user before walking it:

- **Broad request** — two levels. First the batch: pick a handful of cases from step 1's list, agree those, and leave the rest for later batches. Then the steps of each case in the batch.
- **One feature, or an update to an existing case** — one level: the steps of that case.

### 4. Walk it on a device

Run the whole scenario yourself on a connected device, from a clean state, exactly as drafted — [Device control](device-control.md), starting from a clean install ([iOS](../references/install-ios.md#clean-install-reset-app-state) · [Android](../references/install-android.md#clean-install-reset-app-state)).

Inspect → act → verify at every step, and correct the draft as reality dictates: a wrong path, a missing prerequisite, an alert you didn't expect, an expected result that isn't actually visible on screen. Fixing it here is cheap; fixing it after the case is saved and run is not.

Move on once the scenario goes through end to end. For a small edit to a case whose runs already show the path working, the runs are evidence enough — walk only the part you changed, or go straight to step 5.

### 5. Save the case

Collapse the validated walkthrough into flows, phrased per [Writing Good Test Cases](../concepts/writing-test-cases.md): one goal per flow, multi-step instructions as a numbered list (one action per line), a concrete expected result on each. Nothing that is already in app context belongs inside a flow.

- **Reusable flows** — pull a repeated prefix (onboarding, login, account setup) into a reusable flow and reference it by `id`; write it to the same quality bar as an inline flow. Steps unique to this case stay inline. Updating a reusable flow propagates to every case that references it.
- **Updating a case** — read the current case first, then patch only the fields that changed.

Commands for both: [Apps, Cases & Flows](../references/apps-cases-and-flows.md).

### 6. Run it with the agent and confirm it passes

Run the saved cases against a build with the noqa agent — [Run saved tests](run-tests.md). Then read the results per step:

- **Passed** — the case is done.
- **Failed on a step you wrote** (wrong wording, missing setup, an expected result the agent can't see) — fix the case and run it again.
- **Failed on the app itself** — you found a real defect. Report it to the user with the failing step and its screenshot; the case stays as written.

### 7. Repeat, then report back

Take the next scenario from step 1's list. Batch the runs where you can — running several saved cases at once is cheaper than one at a time.

When the batch is done, tell the user what was created, what passes, and anything you found broken in the app along the way.

## Editing the text of a validated case

When the case has runs that already show the path working and the change touches only the wording — a title, tags, a clearer instruction, an expected result phrased better — skip the cycle: read the current case, patch the fields that changed ([Apps, Cases & Flows](../references/apps-cases-and-flows.md)), and say what you changed.

Anything that changes what the agent does or what it has to see — a new step, a different path, a new feature inside the case — goes through steps 3–6 like any other case.

## Notes

- Steps 3–6 are a loop, not a straight line — a case that fails on its own wording goes back to step 4, not into the suite.
- If the user only wants part of the cycle — just explore the app, or just run cases that already exist — use the individual workflow instead of this one.
