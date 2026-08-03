# Workflow: Device Control

You drive the device yourself — inspect, then tap/swipe/drag/type. **Prefer describing the target element in plain language** — noqa resolves its on-screen location for you; explicit relative coordinates read from the screen tree are the fallback. Both, and every other command, are in [Actions](../references/actions.md); reading the screen is [Screen](../references/screen.md).

The whole device is available, not one app. You can:
- install a local build and debug that app;
- install apps from the App Store / Google Play or TestFlight and work with them;
- do something in the browser;
- work in any app on the device, system ones included.

### 1. Connect a device
List available devices and connect the one to work on. See [Devices](../references/devices.md).

### 2. Get the app on the device if you work with a local app

If you do: install the build and note its bundle ID, or confirm it is already installed. If you don't, skip to step 3.

**Get the build** — use the binary or the build command the user gave you; otherwise work it out from the repo (README, build scripts, CI config) and build it: [iOS Native Builds](../references/builds-ios-native.md) · [React Native Builds](../references/builds-react-native.md) · [Android Builds](../references/builds-android.md).
**Install the build** — [iOS](../references/install-ios.md) · [Android](../references/install-android.md).
**Launch the app** — activate it by bundle ID/App ID. See [Actions](../references/actions.md).

Developing a test case (step 4 of [Generate test cases](generate-test-cases.md)) — install from a **clean state**, since every saved run reinstalls the app: [Install iOS](../references/install-ios.md#clean-install-reset-app-state) · [Install Android](../references/install-android.md#clean-install-reset-app-state).

### 3. Inspect the screen and act

Drive the UI as a loop — never fire an action blind:

1. **Inspect** — read the current screen to confirm the target is present and the UI is in the expected state. See [Screen](../references/screen.md).
2. **Act** — perform one action, preferably by describing the target element; fall back to explicit relative coordinates (`0–1000`) only if grounding mislocates. See [Actions](../references/actions.md).
3. **Verify** — inspect the screen again to confirm the state changed as expected, capturing a screenshot for a visual check if needed. See [Screen](../references/screen.md).
4. Repeat for the next action.

App lifecycle, `open-url`, and alert handling are covered in [Actions](../references/actions.md).

Some things you cannot pass on your own — a Face ID / Touch ID or fingerprint prompt, a multi-finger or timing-sensitive gesture, a CAPTCHA. Don't retry them blindly: say what is blocking you and ask the user to do that step on the device. For biometrics, offer the simpler way out — turning Face ID / Touch ID off for the app, or on the device entirely.
