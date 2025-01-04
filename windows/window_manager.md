# Window Manager
This component handles the input for closing and showing windows, it's automatically generated when necessary.

## Properties

| Name | Returns | Summary |
| ------------- | ------------- |
| escapedUsed | bool | Gets a value indicating whether the escape input was used to hide a window in this frame. |
| escapeInputName | string | Gets the escape input name. |
| Instance | UIWindowManager | Gets the current instance of the window manager. |

## Code Usage

You can access the window manager at any time by the Instance static property.

```
if (UIWindowManager.Instance != null)
{
    // Code goes here...
}
```
