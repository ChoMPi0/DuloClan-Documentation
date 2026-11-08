# Step Bar
A bar split into steps with separators and an optional bubble showing the current step (quest progress, tutorials, level progress).

## Setup
1. Add **Step Bar** to a RectTransform.
2. Assign the **Fill Image** (an Image of type Filled).
3. Set **Steps Count** and **Current Step**.
4. Assign the **Separator Sprite** and **Separator Sprite Active** (used for completed steps). **Separator Auto Size** uses the sprite size; turn it off to set **Separator Size**.
5. Optionally assign a **Bubble Rect** with a **Bubble Text** and **Bubble Offset**; it follows the current step.

Rebuild the steps from the inspector after changing the step count.

## Fill Overrides
By default every step fills an equal part of the bar. The **Override Fill List** sets a custom fill amount for specific steps.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GoToStep(int) | void | Moves to the given step. |
| GetStepFillAmount(int) | float | The fill amount for a step. |
| UpdateFillImage() | void | Re-applies the fill. |
| UpdateBubble() | void | Repositions the bubble and updates its text. |
| RebuildSteps() | void | Rebuilds the separators. |
| GetOverrideFillList() | List&lt;StepFillInfo&gt; | Gets the fill overrides. |
| SetOverrideFillList(List&lt;StepFillInfo&gt;) | void | Sets the fill overrides. |

The `step` property gets or sets the current step.

## Code Usage
```
[SerializeField] private UIStepBar m_QuestProgress;

public void OnObjectiveCompleted()
{
    this.m_QuestProgress.step = this.m_QuestProgress.step + 1;
}
```
