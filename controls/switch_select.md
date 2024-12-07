# Switch Select
This component is a select field with previous and next buttons.

## Options
You can add new options to the switch select via the editor text area separated by new line.

Adding options via code is done with the **AddOption(string optionValue)** and **AddOptionAtIndex(string optionValue, int index)** methods.

```
[SerializeField] private UISwitchSelect m_SelectField;

protected void Start()
{
    if (this.m_SelectField == null)
        return;

    // Add new option
    this.m_SelectField.AddOption("My new option");
}
```

Removing options via code is done with the **RemoveOption(string optionValue)** and **RemoveOptionAtIndex(int index)** methods.

```
[SerializeField] private UISwitchSelect m_SelectField;

protected void Start()
{
    if (this.m_SelectField == null)
        return;

    // Remove the first option
    this.m_SelectField.RemoveOptionAtIndex(0);
}
```

Selecting options via code is done with the **SelectOption(string optionValue)** and **SelectOptionByIndex(int index)** methods.

```
[SerializeField] private UISwitchSelect m_SelectField;

protected void Start()
{
    if (this.m_SelectField == null)
        return;

    // Set the selected option
    this.m_SelectField.SelectOption("My new option");
}
```

## Events
The select field has an **onChange** event which will be invoked whenever the selected option is changed.

Here is an example of an eligible method for the change event.

```
public void OnSelectedOption(int index, string option)
{
    Debug.Log("Selected option: " + option);
}
```
