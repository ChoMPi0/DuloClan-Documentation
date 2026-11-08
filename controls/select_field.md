# Select Field
A dropdown select field. It extends the Unity Toggle: the field is "on" while its options list is open. Use it for settings such as resolution, quality or language.

## Setup
1. Add **Select Field** (UI/Select Field) to a GameObject. An Image is required and is used as the target graphic.
2. Assign a TextMeshPro text to **Label Text**. It shows the selected option.
3. Enter the options in the **Options** text area, one per line.
4. Pick the **Default option**.
5. Style the list in the **List Layout**, **Option Layout** and **Option Background Layout** sections.
6. Hook a method to **On Change**.

The list is created when it opens and destroyed when it closes. An invisible blocker covers the screen while the list is open; clicking it closes the list.

## Inspector Settings

### Field

| Property | Summary |
| --- | --- |
| **Options** | One option per line. |
| **Default option** | The option selected by default. |
| **Direction** | Where the list opens: Auto, Down or Up. Auto opens up when the list would go below the bottom of the screen. |
| **Interactable** | Can the field be used. |
| **Label Text** | Text that shows the selected option. |
| **Select Field Layout** | Transition of the field itself (Color Tint, Sprite Swap or Animation). The color, sprite and trigger blocks have extra Active states used while the list is open. |
| **Navigation** | Standard Selectable navigation. |

### List (List Layout)

| Property | Summary |
| --- | --- |
| **Sprite**, **Sprite Type**, **Sprite Color** | The list background. |
| **Margin** | Offsets the list position and width relative to the field. |
| **Padding**, **Spacing** | Padding inside the list and spacing between options. |
| **Transition** | None, Fade or Animation. Fade uses **Duration**. Animation uses an **Animator Controller** with **Open Trigger** and **Close Trigger**. |

### Scroll (Scrolling Properties)

| Property | Summary |
| --- | --- |
| **Allow Scroll Rect** | Use a scroll rect for long lists. |
| **Minimum Options** | The number of options required to use the scroll rect. |
| **List Height** | The list height when scrolling. |
| **Movement Type**, **Elasticity**, **Inertia**, **Deceleration Rate**, **Scroll Sensitivity** | Passed to the ScrollRect. |
| **Scroll Bar Prefab**, **Scroll Bar Spacing** | The scrollbar and its spacing. |

### Option Text (Option Layout)

| Property | Summary |
| --- | --- |
| **Font**, **Font size**, **Font style**, **Color Normal** | Option text style. **Font** is a TMP font asset; empty uses the default font. |
| **Padding** | Padding around the option text. |
| **Effect Type** | None, Shadow or Outline, with **Color**, **Distance** and **Use graphic alpha**. Applied as a shared TMP material ([Text Utility](../miscellaneous/text_utility.md)). |
| **Transition** | None or CrossFade text colors per state. |

### Option Background (Option Background Layout)

| Property | Summary |
| --- | --- |
| **Sprite**, **Sprite Type**, **Sprite Color** | The option background. |
| **Transition** | Color Tint, Sprite Swap or Animation for the option background. |
| **Option Hover Overlay** | Sprite, color and color transition shown on hover. |
| **Option Press Overlay** | Sprite, color and color transition shown on press. |

### Separators (List Separator Layout)

| Property | Summary |
| --- | --- |
| **Sprite**, **Sprite Type**, **Sprite Color** | The separator between options. No sprite means no separators. |
| **Override Height** | Separator height. 0 uses the sprite size. |
| **Start Separator** | Adds a separator before the first option. |

## Keyboard and Gamepad
- Submit opens and closes the list (Toggle behavior).
- While the list is open, Up and Down select the previous or next option. The selection applies right away and **On Change** fires.
- Cancel closes the list.
- When the list closes, EventSystem focus returns to the UI object that was selected before it opened. The chosen option stays selected.

## Events
`onChange` is a `UnityEvent<int, string>`. It passes the option index and the option value.

```
public void OnSelectedOption(int index, string option)
{
    Debug.Log("Selected option: " + option);
}
```

`onTransition` is a `UnityEvent<UISelectField.VisualState, bool>`. It passes the visual state and whether the transition is instant. It is not shown in the inspector; add listeners from code. The **Select Field - Transition** component uses it.

```
public void OnSelectTransition(UISelectField.VisualState state, bool instant)
{
    Debug.Log("Select field state: " + state);
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Open() | void | Opens the list. |
| Close() | void | Closes the list. |
| AddOption(string) | void | Adds an option at the end. |
| AddOptionAtIndex(string, int) | void | Inserts an option. An index past the end adds it at the end. |
| RemoveOption(string) | void | Removes an option and re-validates the selection. |
| RemoveOptionAtIndex(int) | void | Removes the option at the index and re-validates the selection. |
| ClearOptions() | void | Removes all options. |
| SelectOption(string) | void | Selects an option by value (case insensitive). |
| SelectOptionByIndex(int) | void | Selects an option by index. |
| GetOptionIndex(string) | int | The option index, or -1 if not found (case insensitive). |
| ValidateSelectedOption() | void | Selects the first option if the selected one no longer exists. |
| PositionListForDirection(UISelectField.Direction) | void | Positions the open list up or down. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| options | List&lt;string&gt; | The options list (get only). |
| value | string | The selected option. Setting it calls SelectOption. |
| selectedOptionIndex | int | The index of the selected option, or -1. |
| IsOpen | bool | Is the list open. |
| direction | UISelectField.Direction | Where the list opens. |

Prefer the Add/Remove/Clear methods over editing `options` directly. Editing the list directly skips the option list change hook, and removing the selected option that way leaves it selected.

## Code Usage
Fill the field with the supported resolutions and apply the selected one.

```
using UnityEngine;
using DuloGames.UI;

public class ResolutionSelect : MonoBehaviour
{
    [SerializeField] private UISelectField m_SelectField;

    protected void Start()
    {
        if (this.m_SelectField == null)
            return;

        // Clear the options
        this.m_SelectField.ClearOptions();

        // Add the supported resolutions
        foreach (Resolution res in Screen.resolutions)
            this.m_SelectField.AddOption(res.width + "x" + res.height + " @ " + res.refreshRateRatio.value + " Hz");

        // Select the current resolution
        Resolution current = Screen.currentResolution;
        this.m_SelectField.SelectOption(current.width + "x" + current.height + " @ " + current.refreshRateRatio.value + " Hz");
    }

    protected void OnEnable()
    {
        if (this.m_SelectField != null)
            this.m_SelectField.onChange.AddListener(OnSelectedOption);
    }

    protected void OnDisable()
    {
        if (this.m_SelectField != null)
            this.m_SelectField.onChange.RemoveListener(OnSelectedOption);
    }

    private void OnSelectedOption(int index, string option)
    {
        Resolution res = Screen.resolutions[index];

        if (res.Equals(Screen.currentResolution))
            return;

        Screen.SetResolution(res.width, res.height, FullScreenMode.FullScreenWindow, res.refreshRateRatio);
    }
}
```

Remove, insert and select options.

```
[SerializeField] private UISelectField m_SelectField;

public void Example()
{
    // Remove the first option
    this.m_SelectField.RemoveOptionAtIndex(0);

    // Insert an option at the top and select it
    this.m_SelectField.AddOptionAtIndex("Custom", 0);
    this.m_SelectField.SelectOptionByIndex(0);
}
```
