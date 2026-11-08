# Window ID
`UIWindowID` is the list of predefined IDs for a [Window](window.md). Pick one in the window's **ID** field to find the window from code with `UIWindow.GetWindow(UIWindowID)`.

## Values

| Name | Value |
| --- | --- |
| None | 0 |
| Custom | 1 |
| Settings | 2 |
| GameMenu | 3 |
| ModalBox | 4 |
| Character | 5 |
| Inventory | 6 |
| SpellBook | 7 |
| Dialog | 8 |
| QuestLog | 9 |
| Vendor | 10 |

The [Modal Box](../modal_box/modal_box.md) sets its window ID to **ModalBox** itself.

## Custom ID
Choose **Custom** to show the **Custom ID** field and type any integer. Find the window with `UIWindow.GetWindowByCustomID(int)`.

If **Custom ID** is 0, the window assigns itself the next unused custom ID on Start.

## Adding your own IDs
Open `UIWindowID.cs` and add a new entry at the end with the next number:

```
Vendor = 10,
Crafting = 11
```

Keep the values sequential and do not change or reorder the existing ones. The ID is saved as a number, so changing an existing value changes the ID of windows already set up, and the window inspector expects the enum values to match their position in the list.

## Code Usage
```
public void OpenInventory()
{
    UIWindow window = UIWindow.GetWindow(UIWindowID.Inventory);

    if (window != null)
        window.Show();
}
```
