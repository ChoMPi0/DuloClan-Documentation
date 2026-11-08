# Display Value
Writes the value of a progress bar or slider to a **Text**, for example "75%". Use **UI Progress Bar Display Value** for a [Progress Bar](../bars/progress_bar.md) and **UI Slider Display Value** for a **Slider**.

## Setup
1. Add **UI Progress Bar Display Value** or **UI Slider Display Value** to an object.
2. Assign the **Bar** (or **Slider**). If left empty, the component uses the one on the same object.
3. Assign the **Text** that shows the value.
4. Choose the **Display** mode, the number **Format** and the **Append** string.

The text is updated when the component is enabled and every time the value changes.

## Properties

| Name | Default | Summary |
| --- | --- | --- |
| Bar / Slider | none | The source of the value. |
| Text | none | The Text that receives the value. |
| Display | Percentage | **Percentage** multiplies the value by 100. **Raw** shows the value as it is. |
| Format | `0` | A .NET numeric format string, for example `0`, `0.0` or `#,0`. |
| Append | `%` | Text added after the number. Clear it when using **Raw**. |

A progress bar's value is its fill amount (0 to 1), so **Raw** shows a number between 0 and 1. A slider's **Raw** value uses the slider's own **Min Value** / **Max Value** range.

## Object Oriented Methods

| Name | Returns | Summary |
| --- | --- | --- |
| SetValue(float) | void | Formats the value and writes it to the text. Called automatically on change. |
