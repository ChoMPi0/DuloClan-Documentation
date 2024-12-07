# Progress Bar
This component is used for simple progress bars such as Health Bar, Cast Bar etc.

## Types
The progress bar has following types: Filled, Resize and Sprites.

## Steps
The steps setting allows you to assign a fixed amount of steps the bar can fill in.

## Events
The progress bar has an onChange event which will be invoked whenever the bar fill amount is changed.

Here is an example of an eligible method for the change event.

```
public void BarOnChange(float fillAmount)
{
    Debug.Log(fillAmount);
}
```

## Code Usage
```
[SerializeField] private UIProgressBar m_ProgressBar;

public void ChangeFillAmount()
{
    if (this.m_ProgressBar == null)
        return;

    // Change the fill amount
    this.m_ProgressBar.fillAmount = 0.5f;
}
```
