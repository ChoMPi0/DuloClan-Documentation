# Utility
This class contains static methods used for various purposes.

## Bring to Front
When using the **BringToFront** method of this class, you are able to keep some ui elements always on top with the [Always On Top](always_on_top.md) component.

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| BringToFront(GameObject) | void | Brings the game object to the front. |
| BringToFront(GameObject, bool) | void | Brings the game object to the front while specifing if re-parenting is allowed. |
| FindInParents<T>(GameObject) | T | Finds the component in the game object's parents. |
