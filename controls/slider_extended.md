# Slider Extended
This component is just like the regular slider but with the addition of options.

## Options
You can add new options to the slider via the editor text area separated by new line.

You can set the selected option by changing the value property.

```
[SerializeField] private UISliderExtended m_Slider;

public void Start()
{
    if (this.m_Slider == null)
        return;

    this.m_Slider.value = 1f;
}
```

## Events
The slider has an **onValueChanged** event which will be invoked whenever the value is changed.

Here is an example of an eligible method for the change event.

```
public void OnValueChanged(float value)
{
    Debug.Log("Slider value: " + value);
}
```
