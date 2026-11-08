# Switch Select
A select field with previous and next buttons instead of a dropdown list. Use it where a dropdown would not fit, or for short lists such as difficulty or language.

## Setup
1. Add **Switch Select Field** (UI/Switch Select Field) to a GameObject.
2. Assign the **Label Text**, **Prev Button** and **Next Button**.
3. Enter the options in the **Options** text area, one per line.
4. Pick the **Selected option**.
5. Hook a method to **On Change**.

The buttons wrap around: next on the last option selects the first, previous on the first selects the last.

## Inspector Settings

| Property | Summary |
| --- | --- |
| **Label Text** | Text that shows the selected option. |
| **Prev Button** | Selects the previous option. |
| **Next Button** | Selects the next option. |
| **Options** | One option per line. |
| **Selected option** | The selected option. |

## Events
`onChange` is a `UnityEvent<int, string>`. It passes the option index and the option value. It fires only when the selection changes.

```
public void OnSelectedOption(int index, string option)
{
    Debug.Log("Selected option: " + option);
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| AddOption(string) | void | Adds an option at the end. |
| AddOptionAtIndex(string, int) | void | Inserts an option. An index past the end adds it at the end. |
| RemoveOption(string) | void | Removes an option and re-validates the selection. |
| RemoveOptionAtIndex(int) | void | Removes the option at the index and re-validates the selection. |
| SelectOption(string) | void | Selects an option by value (case insensitive). |
| SelectOptionByIndex(int) | void | Selects an option by index. |
| GetOptionIndex(string) | int | The option index, or -1 if not found (case insensitive). |
| ValidateSelectedOption() | void | Selects the first option if the selected one no longer exists. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| options | List&lt;string&gt; | The options list (get only). |
| value | string | The selected option. Setting it calls SelectOption. |
| selectedOptionIndex | int | The index of the selected option, or -1. |

Adding options does not change the label. Select an option after filling the list.

## Code Usage
```
[SerializeField] private UISwitchSelect m_Difficulty;

protected void Start()
{
    if (this.m_Difficulty == null)
        return;

    // Add options
    this.m_Difficulty.AddOption("Easy");
    this.m_Difficulty.AddOption("Normal");
    this.m_Difficulty.AddOption("Hard");

    // Remove the first option
    this.m_Difficulty.RemoveOptionAtIndex(0);

    // Select an option
    this.m_Difficulty.SelectOption("Normal");

    // Listen for changes
    this.m_Difficulty.onChange.AddListener(OnDifficultyChanged);
}

private void OnDifficultyChanged(int index, string option)
{
    Debug.Log("Difficulty: " + option);
}
```
