# Input Event
This component is used to capture and use input events.

## Simple usage
Attach the component to any GameObject and enter the **Input Name**.

You can manage your inputs from the editor's input manger, under the editor top menu Edit -> Project Settings -> Input.

Assign a method to be used with one or more of the available events.

## Events
The input event component has three events, **On Button**, **On Button Down** and **On Button Up**.

Here is an example of eligible methods for the events.

```
public void OnButton() { }

public void OnButtonDown() { }

public void OnButtonUp() { }
```
