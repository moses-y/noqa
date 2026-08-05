# Actions

Everything you do to the device: gestures, text input, app lifecycle, links, alerts. All of it is `noqa action <command>`.

## Targeting an element

Always `noqa screen` first, then take the first option that works:

1. **Element is in the tree** → tap its `relative_bbox_2d` center: `((xmin+xmax)/2, (ymin+ymax)/2)`, relative `0–1000`. Costs nothing.
2. **Tree is no good** — times out, empty, far fewer elements than the screen has, no texts/labels, one flat container (Unity, canvas, WebView, ads), or its coordinates did nothing → `-d "<description>"`, noqa grounds it. Free within a daily quota, then credits.
3. **Grounding out of quota/credits** → tell the user the grounding quota is spent and accuracy will drop from here, then `noqa screenshot`, read the position off the image yourself, and tap by coordinates.

Never guess a target. Describe **only what is currently visible**. See [Screen](screen.md).

## Writing a good element description

When you fall back to grounding, its quality depends entirely on the description. Follow these rules:

- **Be specific** — include color, size, icons, and distinctive features (labels, placeholders).
- **Include exact visible text in quotes** — e.g. `"'Continue' button"`, not `"continue button"`.
- **Mention position if relevant** — top/bottom, left/right/center.
- **Describe exactly ONE element** — never use "or", "any", or "similar".
  - ❌ `"'Play as Guest' button or similar option"`
  - ✅ `"green 'Play as Guest' button below the login form"`
- **Make it locatable by a vision model** — clear enough to pick the element out of the screenshot.
- **Describe only what is visible**, not assumptions about the app's UI.

## Tap

Tap the center of the target's box from `noqa screen`. Grounding it instead, describe the element with its exact visible text in quotes, color, and position — `-d` is a shortcut for `--description`.

```bash
noqa action tap --x <x> --y <y> [--double] [--duration <seconds>]
noqa action tap --description "<description>" [--double] [--duration <seconds>]
```
```bash
noqa action tap --x 500 --y 926
noqa action tap --x 500 --y 926 --double
noqa action tap --description "blue 'Login' button at bottom center"
noqa action tap -d "message item in the conversation list" --duration 3
noqa action tap -d "map area in the center of the screen" --double
```

## Swipe

Describe direction + area + expected outcome.

**Swipe direction is the finger's movement, which is the opposite of the scroll direction.** To scroll **down** the page (reveal content below), swipe **up**; to scroll **up** (reveal content above), swipe **down**. Same for horizontal: to move content left, swipe right, and vice versa. Phrase the description by the finger movement.

```bash
noqa action swipe --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
noqa action swipe -d "<description>"
```
```bash
# scroll down to see more items -> finger swipes up
noqa action swipe --x1 500 --y1 700 --x2 500 --y2 300
noqa action swipe -d "swipe up on the product list to reveal more items below"
# scroll back to top -> finger swipes down
noqa action swipe -d "swipe down on the product list to return to the top"
```

## Drag

Describe the source element and the target location.

```bash
noqa action drag --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
noqa action drag -d "<description>"
```
```bash
noqa action drag --x1 200 --y1 500 --x2 800 --y2 500
noqa action drag -d "drag list item 'Groceries' above 'Work' to reorder"
```

## Pinch

Two-finger pinch to **zoom** a surface (map/image/camera/page) or **resize** a selected on-canvas object (sticker/text/shape). `in` = zoom in / enlarge, `out` = zoom out / shrink. Describe the area to zoom or the object to resize; with coordinates, `(x1, y1)`–`(x2, y2)` is that area (top-left → bottom-right) and only its center anchors the gesture.

```bash
noqa action pinch --direction <in|out> --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
noqa action pinch --direction <in|out> -d "<description>"
```
```bash
noqa action pinch --direction in --x1 200 --y1 200 --x2 800 --y2 900
noqa action pinch --direction in -d "map in the center of the screen to reveal street names"
noqa action pinch --direction out -d "photo in the image viewer to fit the whole picture"
noqa action pinch --direction in -d "selected 'Sale' text sticker on the canvas to make it bigger"
```

## Input text

Target the field by its box from `noqa screen`; grounding it instead, describe it by its label or placeholder.

```bash
noqa action input --text <text> --x <x> --y <y>
noqa action input --text <text> -d "<description>"
```
```bash
noqa action input --text "hello world" --x 500 --y 420
noqa action input --text "john@example.com" -d "email input field with placeholder 'Email'"
```

## App lifecycle

```bash
noqa action activate-app --bundle-id com.example.app
noqa action terminate-app --bundle-id com.example.app
noqa action restart-app --bundle-id com.example.app
noqa action background-app
```

## Open a URL

Opens a link on the device: a deep link into an app, or a web address in the browser.

```bash
noqa action open-url --url "myapp://profile"
noqa action open-url --url "https://example.com"
```

## Alert

Prefer the `alert` command over tapping a button when interacting with a system alert.

```bash
noqa action alert --action accept
noqa action alert --action dismiss
```
