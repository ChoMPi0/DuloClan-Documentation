# Letter Spacing
Adds extra space between the characters of a legacy **Text**. Negative values tighten the text.

## Setup
Add **Letter Spacing** to an object with a **Text** and set **Spacing**. The value is in 1/100 of the font size per character, so it scales with the text.

Spacing respects the text alignment (left, center, right), multiple lines, spaces and rich text tags.

## Layout
Letter Spacing reports an adjusted preferred width to layout groups and Content Size Fitters so the text box grows with the spacing.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| spacing | float | The extra spacing per character. |

## TextMeshPro
Letter Spacing requires a legacy Text and can't be added to a TextMeshPro text. TextMeshPro has this built in: use **Character Spacing** in the TMP text component.
