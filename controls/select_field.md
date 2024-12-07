# Select Field
This component is a dropdown select field.

## Options
You can add new options to the select field via the editor text area separated by new line.

Adding options via code is done with the **AddOption(string optionValue)** and **AddOptionAtIndex(string optionValue, int index)** methods.

```
[SerializeField] private UISelectField m_SelectField;

protected void Start()
{
    if (this.m_SelectField == null)
        return;

    // Clear the options
    this.m_SelectField.ClearOptions();

    // Add the supported resolutions
    Resolution[] resolutions = Screen.resolutions;

    foreach (Resolution res in resolutions)
    {
        // Add new resolution option
        this.m_SelectField.AddOption(res.width + "x" + res.height + " @ " + res.refreshRate + "Hz");
    }
}
```

Removing options via code is done with the **RemoveOption(string optionValue)** and **RemoveOptionAtIndex(int index)** methods.

```
[SerializeField] private UISelectField m_SelectField;

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
[SerializeField] private UISelectField m_SelectField;

protected void Start()
{
    if (this.m_SelectField == null)
        return;

    Resolution currentResolution = Screen.currentResolution;

    // Set the current resolution as selected
    this.m_SelectField.SelectOption(currentResolution.width + "x" + currentResolution.height + " @ " + currentResolution.refreshRate + "Hz");
}
```

## Events
The select field has an onChange event which will be invoked whenever the selected option is changed.

Here is an example of an eligible method for the change event.

```
public void OnSelectedOption(int index, string option)
{
    Debug.Log("Selected option: " + option);
}
```

## Resolution Select Example
This is a simple component script to handle resolution change.

```
using UnityEngine;

namespace DuloGames.UI
{
    public class Demo_ResolutionSelect : MonoBehaviour {

        [SerializeField] private UISelectField m_SelectField;

        protected void Start()
        {
            if (this.m_SelectField == null)
                return;

            // Clear the options
            this.m_SelectField.ClearOptions();
            
            // Add the supported resolutions
            Resolution[] resolutions = Screen.resolutions;

            foreach (Resolution res in resolutions)
            {
                // Add new resolution option
                this.m_SelectField.AddOption(res.width + "x" + res.height + " @ " + res.refreshRate + "Hz");
            }

            Resolution currentResolution = Screen.currentResolution;

            // Set the current resolution as selected
            this.m_SelectField.SelectOption(currentResolution.width + "x" + currentResolution.height + " @ " + currentResolution.refreshRate + "Hz");
        }

        protected void OnEnable()
        {
            if (this.m_SelectField == null)
                return;

            this.m_SelectField.onChange.AddListener(OnSelectedOption);
        }

        protected void OnDisable()
        {
            if (this.m_SelectField == null)
                return;

            this.m_SelectField.onChange.RemoveListener(OnSelectedOption);
        }

        protected void OnSelectedOption(int index, string option)
        {
            Resolution res = Screen.resolutions[index];

            if (res.Equals(Screen.currentResolution))
                return;

            Screen.SetResolution(res.width, res.height, true, res.refreshRate);
        }
    }
}
```
