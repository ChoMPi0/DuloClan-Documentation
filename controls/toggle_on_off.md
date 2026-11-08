# Toggle On/Off
Adds an on/off switch look to a **Toggle**, either by swapping the sprite or by sliding a knob.

## Setup
1. Add **Toggle On/Off** to an object with a **Toggle**.
2. Assign the **Target** Image (the knob or the background).
3. Choose the **Transition**:
    - **Sprite Swap**: shows **Active Sprite** while the toggle is on.
    - **Reposition**: moves the target between **Inactive Position** and **Active Position** (anchored positions).

The visual updates whenever the toggle value changes, including from code (`toggle.isOn = true`).
