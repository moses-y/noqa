# Actions

Everything you do to the device: gestures, text input, app lifecycle, links, alerts. All of it is `noqa action <command>`.

## Targeting an element

Gestures (`tap`, `swipe`, `drag`, `pinch`, `input`) need a target, and there are two ways to give one.

**By description — the primary way.** Pass `--description`/`-d` in plain language and noqa resolves the element's on-screen location for you (vision grounding); you never compute coordinates. **Free within a daily quota**; beyond it grounding draws on credits.

**By coordinates — the fallback.** Here **you** locate the element and pass its position; noqa resolves nothing. Coordinates are relative (`0–1000`), X left→right, Y top→bottom — read them from the target's `relative_bbox_2d` in `noqa screen` (center = `((xmin+xmax)/2, (ymin+ymax)/2)`). Use them when grounding keeps mislocating a target, or when there is no distinct element to describe (a point on a map or canvas).

**Always `noqa screen` first** — confirm the target is on screen, then describe it or read its coordinates; never guess. Base a description **only on what is currently visible**. See [Screen](screen.md).

**When grounding runs out** — the daily quota is spent or the balance is empty, and the command fails on credits — carry on with coordinates. Tell the user before you switch: coordinates are slower to work with and land wrong more often, and the way back is adding credits or waiting for the daily quota to reset.

## Writing a good element description

The quality of grounding depends entirely on the description. Follow these rules:

- **Be specific** — include color, size, icons, and distinctive features (labels, placeholders).
- **Include exact visible text in quotes** — e.g. `"'Continue' button"`, not `"continue button"`.
- **Mention position if relevant** — top/bottom, left/right/center.
- **Describe exactly ONE element** — never use "or", "any", or "similar".
  - ❌ `"'Play as Guest' button or similar option"`
  - ✅ `"green 'Play as Guest' button below the login form"`
- **Make it locatable by a vision model** — clear enough to pick the element out of the screenshot.
- **Describe only what is visible**, not assumptions about the app's UI.

## Tap

Describe the element with its exact visible text in quotes, color, and position. `-d` is a shortcut for `--description`.

```bash
noqa action tap --description "<description>" [--double] [--duration <seconds>]
noqa action tap --x <x> --y <y> [--double] [--duration <seconds>]
```
```bash
noqa action tap --description "blue 'Login' button at bottom center"
noqa action tap -d "message item in the conversation list" --duration 3
noqa action tap -d "map area in the center of the screen" --double
noqa action tap --x 500 --y 926 --double
```

## Swipe

Describe direction + area + expected outcome.

**Swipe direction is the finger's movement, which is the opposite of the scroll direction.** To scroll **down** the page (reveal content below), swipe **up**; to scroll **up** (reveal content above), swipe **down**. Same for horizontal: to move content left, swipe right, and vice versa. Phrase the description by the finger movement.

```bash
noqa action swipe -d "<description>"
noqa action swipe --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
```
```bash
# scroll down to see more items -> finger swipes up
noqa action swipe -d "swipe up on the product list to reveal more items below"
# scroll back to top -> finger swipes down
noqa action swipe -d "swipe down on the product list to return to the top"
noqa action swipe --x1 500 --y1 700 --x2 500 --y2 300
```

## Drag

Describe the source element and the target location.

```bash
noqa action drag -d "<description>"
noqa action drag --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
```
```bash
noqa action drag -d "drag list item 'Groceries' above 'Work' to reorder"
noqa action drag --x1 200 --y1 500 --x2 800 --y2 500
```

## Pinch

Two-finger pinch to **zoom** a surface (map/image/camera/page) or **resize** a selected on-canvas object (sticker/text/shape). `in` = zoom in / enlarge, `out` = zoom out / shrink. Describe the area to zoom or the object to resize; with coordinates, `(x1, y1)`–`(x2, y2)` is that area (top-left → bottom-right) and only its center anchors the gesture.

```bash
noqa action pinch --direction <in|out> -d "<description>"
noqa action pinch --direction <in|out> --x1 <x1> --y1 <y1> --x2 <x2> --y2 <y2>
```
```bash
noqa action pinch --direction in -d "map in the center of the screen to reveal street names"
noqa action pinch --direction out -d "photo in the image viewer to fit the whole picture"
noqa action pinch --direction in -d "selected 'Sale' text sticker on the canvas to make it bigger"
noqa action pinch --direction in --x1 200 --y1 200 --x2 800 --y2 900
```

## Input text

Describe the field by its label or placeholder.

```bash
noqa action input --text <text> -d "<description>"
noqa action input --text <text> --x <x> --y <y>
```
```bash
noqa action input --text "john@example.com" -d "email input field with placeholder 'Email'"
noqa action input --text "hello world" --x 500 --y 420
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
