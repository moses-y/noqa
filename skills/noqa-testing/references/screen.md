# Screen

Reading what is on the device. Acting on what you find is [Actions](actions.md).

```bash
noqa screen                        # compact UI element tree
noqa screen --full                 # full tree with all attributes, can be very long
noqa screenshot /tmp/screen.png    # save screenshot to file, to visually verify
```

**Always inspect before and after any action to verify the actual UI state.**

**Prefer `noqa screen` first — it is the primary way to read the screen.** The element tree is far cheaper in tokens than an image and is enough to understand the UI in most cases. Only fall back to `noqa screenshot` when the tree is not enough: it comes back empty, it lacks the information you need, or you can't tell what's actually rendered (custom drawing, images, visual layout/overlap). Use the screenshot to fill that gap, not as the default way to look at the screen.

Call `noqa screen` directly — do not pipe through `grep`, `awk`, or any other filter. Parse the raw output yourself. Filtering can hide elements you need.

Each element carries a `relative_bbox_2d="[xmin, ymin, xmax, ymax]"` (relative `0–1000` on both axes) — this is where your tap coordinates come from: center is `((xmin+xmax)/2, (ymin+ymax)/2)`. When the tree gives you nothing to act on, see [Targeting an element](actions.md#targeting-an-element).
