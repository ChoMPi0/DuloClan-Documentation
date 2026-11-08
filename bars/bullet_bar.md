# Bullet Bar
A progress bar made of separate bullets (ammo, combo points, charges). Bullets are laid out horizontally, vertically or in a circle and light up according to the fill amount.

## Setup
1. Add **Bullet Bar** to an empty RectTransform.
2. Choose the **Bar Type**: Horizontal, Vertical or Radial.
3. Assign the **Bullet Sprite** (inactive) and **Bullet Sprite Active**, with their colors.
4. Set **Bullet Count** and, for radial bars, **Distance**, **Angle Min** and **Angle Max**.
5. Use **Fixed Size** / **Bullet Size** to override the sprite size, and **Sprite Rotation** / **Active Position** to fine tune.

The bullets are generated in the editor. Rebuild them from the inspector after changing the layout settings.

## Fill
`fillAmount` (0 to 1) sets how many bullets are active. **Invert Fill** fills from the opposite end.

## Properties and Methods

| Name | Type | Summary |
| --- | --- | --- |
| fillAmount | float | Fill amount from 0 to 1. |
| invertFill | bool | Fill from the opposite end. |
| UpdateFill() | void | Re-applies the fill to the bullets. |
| ConstructBullets() | void | Rebuilds the bullet objects. |

The bullet bar implements `IUIProgressBar`, so it can be used anywhere a [Progress Bar](progress_bar.md) is expected.

## Code Usage
```
[SerializeField] private UIBulletBar m_AmmoBar;

public void SetAmmo(int ammo, int maxAmmo)
{
    this.m_AmmoBar.fillAmount = (float)ammo / maxAmmo;
}
```
