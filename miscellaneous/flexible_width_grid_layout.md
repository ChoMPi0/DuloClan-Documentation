# Flexible Width Grid Layout
A **Grid Layout Group** that calculates the cell width from the available width, so a fixed number of columns always fills the container.

## Setup
1. Add **Flexible Width Grid Layout** to the container instead of a Grid Layout Group.
2. Set **Constraint Count** to the number of columns.
3. Set the **Cell Size** Y (height), **Padding** and **Spacing**.

## Behavior
Before each layout pass the component:
- sets **Constraint** to **Fixed Column Count**,
- sets the **Cell Size** X to `(width - horizontal padding - spacing X * (columns - 1)) / columns`.

Any **Cell Size** X or **Constraint** value set in the inspector is overwritten. All other Grid Layout Group settings work as usual.
