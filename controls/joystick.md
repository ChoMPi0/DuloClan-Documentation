# Joystick
An on-screen virtual joystick for touch and mouse input.

## Setup
1. Create the joystick background and add **Joystick**.
2. Assign the **Handle** (the knob RectTransform, a child of the joystick).
3. Optionally assign a **Handling Area** (the touch area; defaults to the joystick itself) and an **Active Graphic** that fades in while pressed.
4. Set **Spring** (how fast the handle returns), **Dead Zone** and the **Output Curve**.

Use a Screen Space Overlay canvas, and give the handling area a fixed size (not stretched anchors).

## Reading the Axis
`JoystickAxis` returns a Vector2 from -1 to 1 on each axis, with the dead zone and output curve applied. Read it every frame.

```
[SerializeField] private UIJoystick m_Joystick;
[SerializeField] private float m_Speed = 5f;

void Update()
{
    Vector2 axis = this.m_Joystick.JoystickAxis;
    this.transform.Translate(new Vector3(axis.x, 0f, axis.y) * this.m_Speed * Time.deltaTime);
}
```

## Properties and Methods

| Name | Type | Summary |
| --- | --- | --- |
| JoystickAxis | Vector2 | The current axis value. Setting it moves the handle. |
| SetAxis(Vector2) | void | Sets the axis (clamped to a magnitude of 1). |
| Handle | RectTransform | The handle. |
| HandlingArea | RectTransform | The touch area. |
| ActiveGraphic | Image | Graphic shown while pressed. |
| Spring | float | Return speed of the handle. |
| DeadZone | float | Values below it read as zero. |
