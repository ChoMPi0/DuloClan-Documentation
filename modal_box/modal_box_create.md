# Modal Box Create
This component is used to simplify instantiation of Modal Box, it uses the Modal Box Manager to create the box.

## Simple Usage
Attach the component to any GameObject and assign the required fields.

Fill in the **Text 1** and **Text 2** fields.

Fill in the **Confirm Text** and **Cancel Text** fields.

Assign a Button to the **Hook To Button** field or invoke the **CreateAndShow()** method via code.

Assign the **onConfirm** event.

## Events
The modal box create component has two events, **onConfirm** and **onCancel**.

Here is an example of eligible methods for the events.

```
public void OnConfirm()
{
    Debug.Log("Confirm pressed.");
}

public void OnCancel()
{
    Debug.Log("Cancel pressed.");
}
```

## Code Usage
You can create and show the modal box via code by invoking the **CreateAndShow()** method.
