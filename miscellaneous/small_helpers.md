# Small Helpers
Small components that each do one job. Most are meant to be wired to a UnityEvent in the inspector.

## Scroll Rect Expander
Widens a target when a scrollbar is hidden, and shrinks it back when the scrollbar is shown again. Use it with a **Scroll Rect** whose scrollbar **Visibility** is **Auto Hide**, so the content uses the space of the hidden scrollbar.

1. Add **UI Scroll Rect Expander** to the scrollbar object.
2. Assign the **Target** RectTransform (for example the viewport).
3. Set **Expand Width** to the width added while the scrollbar is hidden.

It reacts only when the scrollbar's own GameObject is deactivated or activated, not when a parent is.

## Input Caret Offset
Moves the caret of a **TMP Input Field** by an offset. Useful when the font makes the default caret look misaligned.

1. Add **UI Input Caret Offset** to an object with a **TMP Input Field**.
2. Set the **Offset**.

The input field creates its caret object at runtime, under its text area. When the component is enabled or the children of the input change, it waits 0.5 seconds and moves the first object below the input whose name contains "caret" by the **Offset**. `ApplyOffset()` is public and can also be called manually; each call adds the offset to the current position.

## Globe Fill Inverter
Sets an **Image** fill amount to `1 - value`. Use it to fill an image from the opposite side of a globe or bar.

1. Add **UI Globe Fill Inverter** and assign the **Image**.
2. Add `UIGlobeFillInverter.OnChange` to a float event, for example a [Progress Bar](../bars/progress_bar.md) **On Change** or a Slider **On Value Changed**.

## Stats Add
Adds 1 to the whole number shown in a TextMeshPro text. Used for "+" buttons on attribute points.

1. Add **UI Stats Add** and assign the **Value Text**.
2. Add `UIStatsAdd.OnButtonPress` to a Button **On Click**.

The text must contain a whole number, otherwise an exception is thrown.

## Text Set Value
Writes a float to the TextMeshPro text on the same object.

1. Add **UI Text Set Value** to an object with a **TextMeshPro - Text (UI)**.
2. Set the **Float Format** (default `0.00`).
3. Add `UITextSetValue.SetFloat` to a float event, for example a Slider **On Value Changed**.
