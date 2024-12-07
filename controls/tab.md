# Tab
This component is just like a regular Toggle with the addition of being able to enable and disabled targetted GameObject.

## Events
The tab has an onValueChanged event which will be invoked whenever the toggle is changed.

Here is an example of an eligible method for the change event.

```
public void OnToggleStateChanged(bool state)
{
    Debug.Log("Tab state: " + state);
}
```
